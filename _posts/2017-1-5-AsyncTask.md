---
layout: post
title: AsyncTask
published: true
categories: Android
---

AsyncTask is a short form for asynchronous task. AsyncTask allows you to perform asynchronous work on your User Interface. By using AsyncTask, you can run blocking operations in a worker thread and publish the results on the User Interface without having to handle threads and/or handlers. As such, it is a helper class around threads and handlers and provides a proper and easy way of handling background operations with the UI Thread.

## Why do we need AsyncTask?

Android Applications are run in a single thread called the main thread. This thread is also commonly referred to as the UI Thread. Because of the single thread model, it is vital to the responsiveness of your application's UI that the UI Thread is not blocked. Therefore, blocking codes in this thread will cause the application's user interface to "hang" and result in the "Application Not Responding" (ANR) dialog. Not only does the UI becomes not responsive, it causes the entire application to crash. 

AsyncTask is not a replacement of Threads and Handlers, but simply a helper class for simple background operations. Threads and Handlers are the traditional ways to run blocking operations in worker threads. However writing threads and handlers can quickly become complicated and also results in less readable codes. This is especially inefficient when the background operations are simple and short. AsyncTask provides a proper and easy way to run simple background operations, and manipulate the UI before, during and after the operation. Hence, if you need to keep threads running for long periods of time, it is recommended you use the various APIs provided by the java.util.concurrent packages such as [Executor](https://developer.android.com/reference/java/util/concurrent/Executor.html), [ThreadPoolExecutor](https://developer.android.com/reference/java/util/concurrent/ThreadPoolExecutor.html) and [FutureTask](https://developer.android.com/reference/java/util/concurrent/FutureTask.html).

## How to create a AsyncTask? 

AsyncTask has to be <u><b>subclassed</b></u> in order to be used. In another words, you have to create your own class and extends AsyncTask. You will have the liberty to give a name to this asynchronous task that you will be performing in your application. Please pay extra attention to the rules of using AsyncTask below. AsyncTask is simple to use such that many a times we overlook the rules to abide by when using it. By keeping the rules in mind, it can save you alot of debugging time by knowing the proper way to use AsyncTask.

### 3 Generics of AsyncTask <Params, Progress, Result>

When you subclass AsyncTask, you will need to declare the generics of 3 variables. These 3 variables are Params, Progress and Results. By defining the generics of these 3 variables, your AsyncTask will know what are the variable types it is working with.

{% highlight java %}
private class MyTask 
  extends AsyncTask<Params, Progress, Results>{ ... }
{% endhighlight %}

- Params: Params is the type of parameters that is going to be sent to the AsyncTask to be worked on. For example, if your AsyncTask is a task that request information from multiple URLs, then your Params should be the generic URL.
- Progress: Progress is the type of parameters that is going to be published on the UI thread as indicators to Users. For example, if you have a progress bar, and you want to display percentage done, then your Progress should be the generic Integer or Double. If you want to display progress in messages, then Progress should be the generic String. 
- Results: Results is the type of parameters that is going to be returned by the background operation. For example, your task returns a JSONObject after calling a REST API in the background operation. Then your Results should be the generic JSONObject. If your task returns a count of the number of results received, then your Results should be the generic Integer. 

An example of a subclassed AsyncTask that is properly declared is as follows:

{% highlight java %}
private class DownloadPictures 
  extends AsyncTask<URL, Integer, Integer>{ ... }
{% endhighlight %}

Note that sometimes some of these variables are not used by the asynchronous task. To mark a type as unused, simple use the type Void.

{% highlight java %}
private class DownloadPictures 
  extends AsyncTask<Void, Void, Void>{ ... }
{% endhighlight %}

### Protected Methods 

These protected methods provides a proper way of interaction between the AsyncTask and the User Interface. By subclassing AsyncTask, minimally you have to override the method doInBackground(...). More often than not, onPostExecute is also overriden in order to use the results from the asynchronous task.

{% highlight java %}
private class MyTask 
  extends AsyncTask<Params, Progress, Results>{ 
    
  @Override
  protected Results doInBackground(Params... params) { 
    // Blocking logic
  }
}
{% endhighlight %}

The methods onPreExecute, onPostExecute, onProgressUpdate and publishProgress run on the UI thread, meaning that it is safe to manipulate the UI in these methods. The method doInBackground runs on a worker thread and therefore is executed in the background and it is not advisable to manipulate the UI in this method. 

#### onPreExecute()

{% highlight java %}
@Override
protected void onPreExecute(){ 
}
{% endhighlight %}

This method is invoked right before the task is executed. This method runs on the UI Thread and is normally used as a UI setup for the task. Some examples are displaying the progress bar or changing the button view before executing the task. It tells the user that the task is currently running.

#### doInBackground(Params... params)

{% highlight java %}
@Override
protected Results doInBackground(Params... params){ 
  // Blocking logic
}
{% endhighlight %}

This method is invoked on the worker thread immediately after onPreExecute() finishes executing. This method is where all the blocking logic is written and to be run in a worker thread. As the logic in this step is executed in a separate thread, do not manipulate the UI directly in this method. 

The parameters of the AsyncTask is passed into this step as params. The return of this method is passed into the last step of AsyncTask, onPostExectute. Therefore the results of AsyncTask must be return at this step and the results must be of the generic type declared when subclassing the AsyncTask.

Manipulation of the UI can be done through calling the publishProgress method. This provides a way to update your UI such as progress bar or displaying messages. publishProgress is executed on the UI thread and therefore is the proper way to manipulate the UI. Note that publishProgress takes in the generic declared when subclassing the AsyncTask.

#### publishProgress(Progress... progress)

This method is called only in the doInBackground method. It provides a way for the method doInBackground to manipulate the UI. By calling publishProgress, the onProgressUpdate is invoked and the parameters that is passed into the publishProgress will be passed into onProgressUpdate.

{% highlight java %}
@Override
protected Results doInBackground(Params... params){ 
    for (int i=0 ; i<100 ; i++){
        publishProgress(i);
    }
}
{% endhighlight %}

Through using publishProgress, the worker thread can access the UI thread and manipulate the UI safely. Note that the units of progress is the generic declared when subclassing the AsyncTask.

#### onProgressUpdate(Progress... progress)

{% highlight java %}
@Override
protected void onProgressUpdate(Progress... progress){ 
}
{% endhighlight %}

onProgressUpdate is invoked after a call to publishProgress is made. The timing of execution of this method after publishProgress is called is undefined. There is no guarantee that it will be called immediately but depends on how the system deals with multithreading.  

onProgressUpdate is invoked on the UI thread and is used to display any form of progress in the UI while the background computation is still in progress. For example, to animate and update the progress bar or display messages on the logcat. 

#### onPostExecute(Result result)

{% highlight java %}
@Override
protected void onPostExecute(Result result){ 
}
{% endhighlight %}

onPostExecute is invoked immediately after doInBackground finishes executing. This method is executed on the UI thread and is normally used to update the UI to indicate that the asynchronous task has finished execution. For example, to hide the progress bar or change the information on the UI with the newly acquired information. 


### Using AsyncTask

AsyncTask must be subclassed to be used. The subclassed AsyncTask can be a nested class or simply a class on its own. In order to start the AsyncTask, the execute method must be called. The paramaters is passed into the task through the execute method.

This is an example of a nested AsyncTask class in an Android Activity.

{% highlight java %}
public class MainActivity extends AppCompatActivity{

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
  }

  public void onClick(View view){
    new DownloadImageTask().execute("http://example.com/image.png");
  }

  private class DownloadImageTask extends AsyncTask<String, Void, Bitmap> {

    /** The system calls this to perform work in
     * the UI thread before doInBackground is executed */
    protected void onPreExecute(){
      showLoadingDialog();
    }

    /** The system calls this to perform work in a 
     * worker thread and delivers it the parameters 
     * given to AsyncTask.execute() */
    protected Bitmap doInBackground(String... urls) {
      return loadImageFromNetwork(urls[0]);
    }

    /** The system calls this to perform work in 
     * the UI thread and delivers the result from 
     * doInBackground() */
    protected void onPostExecute(Bitmap result) {
      ImageView mImageView = findViewById(R.id.mImageView)
      mImageView.setImageBitmap(result);
      hideLoadingDialog();
    }
  }
}
{% endhighlight %}

This is an example of a AsyncTask class separate from the Android Activity class

{% highlight java %}
public class DownloadImageTask extends AsyncTask<String, Void, Bitmap> {

  /** The system calls this to perform work in
    * the UI thread before doInBackground is executed */
  protected void onPreExecute(){
    showLoadingDialog();
  }

  /** The system calls this to perform work in a 
    * worker thread and delivers it the parameters 
    * given to AsyncTask.execute() */
  protected Bitmap doInBackground(String... urls) {
    return loadImageFromNetwork(urls[0]);
  }

  /** The system calls this to perform work in 
    * the UI thread and delivers the result from 
    * doInBackground() */
  protected void onPostExecute(Bitmap result) {
    ImageView mImageView = findViewById(R.id.mImageView)
    mImageView.setImageBitmap(result);
    hideLoadingDialog();
  }
}
{% endhighlight %}

{% highlight java %}
public class MainActivity extends AppCompatActivity{

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
  }

  public void onClick(View view){
    new DownloadImageTask().execute("http://example.com/image.png");
  }
}
{% endhighlight %}

Both methods are viable and provides neat and readable codes. Note that AsyncTask <b><u>MUST</u></b> be executed in the UI thread; in another words, in the Activity or Fragment class. 

Refer to the rules below for more rules to abide by in order for AsyncTask to work properly.

### Cancelling AsyncTask, 

An AsyncTask can be cancelled by calling the cancel(boolean) method after execute(params...) method is called. After invoking cancel(boolean), subsequent calls to isCancelled() will return true. 

If cancel(boolean) method is called, the onCancelled(Results) method is invoked instead of onPostExecute(Results). Therefore the onCancelled(Results) is only invoked after doInBackground has finished executing. In order to cancel the AsyncTask as soon as possible after cancel(boolean) is called, you should always check the return value of isCancelled() in doInBackground method. The moment isCancelled() returns true, return the method and onCancelled(Results) is invoked right after. 

Calling cancel(boolean) will fail if the task has already completed, already been cancelled, or could not be cancelled for some other reasons. If the task is successfully cancelled and the task has not started, then the task should never run. If the task has already started, then whether the worker thread running the background logic can be interrupted will depend on the boolean parameter passed into the method.

The boolean parameter in cancel(boolean) is a flag for "mayInterruptIfRunning". In simple terms, it means that if mayInterruptIfRunning is true; the task will be interrupted and cancelled if cancel is called when the task is running. If mayInterruptIfRunning is false; the task will not be interrupted and cancelled if cancel is called when the task is running. 

{% highlight java %}
public class MainActivity extends AppCompatActivity{

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
  }

  public void onClick(View view){
    new DownloadImageTask().execute("http://example.com/image.png");
  }

  private class DownloadImageTask extends AsyncTask<String, Void, Bitmap> {    
    ...
    protected Bitmap doInBackground(String... urls) {
      return loadImageFromNetwork(urls[0]);
    }

    protected void onPostExecute(Bitmap result) {
      ImageView mImageView = findViewById(R.id.mImageView)
      mImageView.setImageBitmap(result);
      hideLoadingDialog();
    }

    protected void onCancelled(Bitmap result){
      hideLoadingDialog();
    }
  }
}
{% endhighlight %}

### Threading rules of using AsyncTask

The following rules should be abided to guarantee the class to work as expected.

1. AsyncTask instance must be created on the UI Thread.
2. execute(Params... params) must be invoked on the UI Thread.
3. Do not call any of the protected methods manually.
4. The task can only be executed once. If execute is called subsequent times, an exception will be thrown.
5. Starting with HONEYCOMB, all AsyncTask instances are executed on a single worker thread. If parallel execution of the instances are required, use executeOnExecutor(java.util.concurrent.Executor, Object[]) with THREAD_POOL_EXECUTOR.

## Conclusion

AsyncTask is a simple helper class for running background operations. Tasks should generally be shortlived and once off. If there is a need for long running worker threads, Threads and Handlers should be used instead. The methods in AsyncTask class are guaranteed to be synchronised and one method will be called once at a time. 

By using AsyncTask, we can write clean and readable codes and not fall into the complexity of Threads/Handlers or Thread safety in an Android Application. 

## Reference
1. https://developer.android.com/reference/android/os/AsyncTask.html
2. https://developer.android.com/guide/components/processes-and-threads.html











