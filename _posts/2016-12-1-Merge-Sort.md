---
layout: post
title: Merge Sort
published: true
categories: Programming
---

Merge sort is a classic example of how the concept Divide and Conquer can improve the run time of your algorithm. Sorting can become a complex problem when you have a huge data set and you dont have all the time in the world to let your machine slowly sort the list. Therefore great minds along the years have come up with many sorting methods such as insertion sort, quick sort, counting sort, radix sort and the list goes on and on. 

In the comparison models for sorting, we have sorting methods such as insertion sort and merge sort where it some point in the algorithm, we always have one main object compared to the others. Comparision sort have a fundamental requirement of O(n log n), and one such algorithm is Merge Sort, which we are going to discuss in this article. 

## Complexity

There are three parts in the Divide and Conquer approach. Note that Divide and Conquer is a recursive approach and that the 3 parts are recursively done to break down the problem into further subproblems.

1. **Divide**. We first have to divide the problem into sub problems. Therefore dividing an array of size n into 2 subarrays of size (n/2).
2. **Conquer**. In this step, we sort each of the subarrays in itself.
3. **Merge**. In order to merge the sorted subarrays, we interatively compare the first elements of the subarrays and put them into one array.

So how does Merge Sort runs in O(n log n)? 

#### Divide and Merge steps

In order to divide the array into 2 subarrays, we have to intialise 2 subarrays of size n/2. Then iterate through the entire array and clone the first half into the first subarray and the second half into the second subarray. In this step, every division takes O(n) steps.

{% highlight java %}
private T[][] divide(T[] toDivide){
  T[] toSort1 = (T[]) new Comparable[toDivide.length/2];
  T[] toSort2 = (T[]) new Comparable[toDivide.length - toSort1.length];
  for (int i=0 ; i<toSort1.length ; i++){
    toSort1[i] = toDivide[i];
  }
  for (int i=0 ; i<toSort2.length ; i++){
    toSort2[i] = toDivide[toSort1.length+i];
  }
	
  T[][] divided = (T[][]) new Comparable[2][];
  divided[0] = toSort1;
  divided[1] = toSort2;
  return divided;
}
{% endhighlight %}

Merging subarrays also take on similar approach. We have to interate through both of the sorted subarrays. Since the subarrays are already sorted, we can simply compare the first of both subarrays and put them into one single merged array. Therefore every merge also takes O(n) steps.

{% highlight java %}
private T[] merge(T[] toMerge1, T[] toMerge2){
  T[] toSort1 = toMerge1;
  T[] toSort2 = toMerge2;
  T[] toMerge = (T[]) new Comparable[toMerge1.length+toMerge2.length];
	
  int[] indexes = {0, 0};
  for (int i=0 ; i<toMerge.length ; i++){
    if ( indexes[0]>toSort1.length-1 ){
      toMerge[i] = toSort2[indexes[1]];
      indexes[1] = indexes[1]+1;
    } else if ( indexes[1]>toSort2.length-1 ){
      toMerge[i] = toSort1[indexes[0]];
      indexes[0] = indexes[0]+1;
    } else{
      if ( compare(toSort1[indexes[0]], toSort2[indexes[1]]) < 0){
        toMerge[i] = toSort1[indexes[0]];
        indexes[0] = indexes[0]+1;
      } else {
        toMerge[i] = toSort2[indexes[1]];
        indexes[1] = indexes[1]+1;
      }
    }
  }
  return toMerge;
}
{% endhighlight %}

Putting together the steps taken to divide and merge, we can assume that it takes O(cn) steps, where c is a constant > 0. 

#### Recursively conquer

We first start from sorting the initial problem. Assuming the time taken for sorting an array of size n takes T(n) time and the time taken to sort an array of size n/2 is T(n/2) and so on and so forth. Eventually we will have n numbers of array of size 1. Naturally if the size of the array is 1, there is no need to sort an array of this size and the number of steps is O(1)

![Recursive conquer]({{ site.baseurl }}/images/2016-12-1-Subproblems.png ){: .center-image }

#### Calculating Complexity

The total steps taken to actually sort the array comes from the dividing and merging steps. So whats left is figuring the total steps taken from dividing and merging the arrays at each level. If dividing and merging an array of size n takes cn steps, dividing and merging an array of size n/2 takes cn/2 steps. However 1 array of size n results in 2 array of size n/2, and therefore 2 array of size n/2 takes 2*cn/2 steps which results back to cn steps. If we continuing examining the levels downwards, every level will total up to cn steps. 

![Recursive conquer]({{ site.baseurl }}/images/2016-12-1-TimeComplexity.png ){: .center-image }

By virtue of dividing the problem into 2 at every level, there will be a total of log n levels of cn step size. Therefore the complexity of solving this problem is O(n log n)

## Implementation

After we have implemented the divide and merge methods, the main sorting method is fairly straightforward. We just have to recursively split the array into subarrays and call the sort method on each of them. After that, merge the results and we can return the sorted array.

{% highlight java %}
public T[] sort(){
  array = sort(array);
  return array;
}
private T[] sort(T[] toSort){
  if (toSort.length < 2){
    // base case
    return toSort;
  }
	
  T[][] divide = divide(toSort);
  divide[0] = sort(divide[0]); 
  divide[1] = sort(divide[1]);		
  toSort = merge(divide[0], divide[1]);

  return toSort;
}
{% endhighlight %}

## Conclusion

Merge sort is a straightforward implementation. It can be very helpful if you need a fairly efficient sorting method quickly implemented. This sorting method can also be easily extended to all data types as long as there is a method of comparison between the data types. My full implementation can be found [HERE][1].

## References

1. <https://www.khanacademy.org/computing/computer-science/algorithms/merge-sort/a/analysis-of-merge-sort>
2. <https://en.wikipedia.org/wiki/Sorting_algorithm>

[1]:{{ site.baseurl }}/downloads/2016-12-1-MergeSort.java


















