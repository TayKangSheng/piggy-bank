---
layout: post
title: Linear Time Sorting
published: true
categories: Programming
---

The usual sorting methods always comprises of comparisons between a[i] and a[j] where a[i] and a[j] are elements of the array to be sorted. Such sorting methods are called comparison sort. Some common examples of such sorting methods are MergeSort, QuickSort and HeapSort. Every comparison based sorting algorithm takes Ω(n log n) on average and cannot be faster than that. Note that we are not making any claims about "good" instances as such instances are for example sorted list where sorting might even take linear time. 

In this article, we are going to discuss faster sorting algorithms that takes linear time. Specifically, we are going to discuss Counting Sort (a more streamlined version of Bucket Sort) and Radix Sort. We will be going through its logic and implementation. **All sorting implementations in this article are for keys of decimal integers**. After knowing the logic, one can expand the sorting methodologies for other key types.

## Counting Sort

Counting sort is a sorting algorithm that works best where the number of unique keys are not significantly large. As there are no need to sort unique keys as they are of similar values, keys of the same value are dropped into buckets and combined at the end. 

The steps are as follows:

1. Create a histogram of the frequency of each unique key.
2. Use arithmetic on the histogram to determine the starting/ending positions of each bucket.
3. Put all keys into their respective positions. 

Implementation of counting sort is simple and straight forward.

{% highlight java %}
public int[] countingSort(int[] toSort){
  // Count the number of buckets 
  // (aka the number of different keys)
  int max = toSort[0];
  for (int i=0 ; i< toSort.length ; i++){
    if (toSort[i] > max){
      max = toSort[i];
    }
  }
  // To account for that the key 0 is also a key.
  max+=1; 
	
  // this array keeps count of the size of each bucket
  int[] count = new int[max]; 
	
  // determine how big each bucket should be.
  // (aka histogram of the frequencies)
  for (int i : toSort){
    count[i]++;
  }
	
  // determine the ending index of each bucket
  // on a single array. (aka offset)
  for (int i=1 ; i<count.length ; i++){
    count[i]+=count[i-1];
  }
	
  // input each entry into a single array at the ending index 
  // of its bucket and minus the ending index by 1
  int[] tempArray = new int[toSort.length];
  for (int i=toSort.length-1 ; i>=0 ; i--){
    tempArray[--count[toSort[i]]] = toSort[i];
  }
  return tempArray;
}
{% endhighlight %}

Counting Sort uses a methodology more similar towards hashing by using the key values to determine the indexes in the array. Therefore unlike to comparison sorts, there is no repetitive comparison to determine the indexes of key values in the array. 

The **time complexity** of this sorting method is O(n + k), where n is the number of keys in the array and k is the number of unique keys. The first for loop to find the maximum number, second for loop to create the histogram of the frequencies and the last for loop where each element is placed in its sorted index takes O(n) time. The thrid for loop loops through the list of unique keys takes O(k) time. Therefore the running time in total for Counting Sort is O(n + k).

Bucket Sort is generally similar to Counting sort both in implementation and time analysis, just that bucket sort requires link list and dynamic arrays or a large amount of preallocated memory to hold the sets of items within each bucket, whereas counting sort stores a single value in each bucket, which is the frequency.  

## Radix Sort

So what if we have a significantly large amount of keys and risk setting up millions of buckets where most buckets will stay empty? Radix Sort is an extension of Counting/Bucket sort where we sort the keys iteratively through the significant digits of the value. 

Lets consider keys of this form: 
{% highlight none %}
d[k-1]d[k-2]....d[2]d[1]d[0]
where 0 <= d[i] < B for some base B
and 	d[k-1] is the Most Significant Digit,
	d[0] is the Least Significant Digit
{% endhighlight %}

There are 2 basic approach to Radix Sort. The first is called the Least Significant Digit Radix Sort where we sort the elements by their least significant digit first, and slowly move up the ranks to the most significant digit. The second is called the Most Significant Digit Radix Sort where we sort the elements by their most significant digit first and slowly move down the ranks to the least significant digit. While sorting the keys at a signicant digit, the process is same or a variation of counting sort.

#### Least Significant Digit Radix Sort

As mention above, the Least Significant Digit Radix Sort sorts the keys by their least significant digit first and slowly move up the ranks to the most significant digit. The algorithm can be done recursively or non-recursively. 

The recursive algorithm is as follows:

{% highlight none %}
int[] toSort;
For i = 0..k where k is the number of digits:
  Sort toSort on i-th digit
{% endhighlight %}

We first have to implement the counting sort on single digits.

{% highlight java %}
private int[] LSDbucketSortOnDigit(int[] toSort, int digit){
  int[] count = new int[10];
  int[] tempArray = new int[toSort.length];
	
  // determine how big each bucket should be.
  for (int i : toSort){
    count[getNthDigit(i, 10, digit+1)]++;
  }
  // determine the ending index of each bucket
  // on a single array. (aka offset)
  for (int i=1 ; i<10 ; i++){
    count[i]+=count[i-1];
  }
  // input each entry into a single array at the ending index 
  // of its bucket and minus the ending index by 1
  for (int i=toSort.length-1 ; i>=0 ; i--){
    tempArray[--count[getNthDigit(toSort[i], 10, digit+1)]] = toSort[i];
  }
  return tempArray;
}
{% endhighlight %}

After implementing counting sort by each digit, the Least Significant Digit Radix Sort is simple to implement.

{% highlight java %}
public int[] radixSort_LSD(int[] toSort){
  int max = toSort[0];
  for (int i : toSort){
    if (max < i){
      max = i;
    }
  }
  return radixSort_LSD(toSort, String.valueOf(max).length()-1);
}
private int[] radixSort_LSD(int[] toSort, int digit){
  if (digit == 0){
    return LSDcountingSortOnDigit(toSort, digit);
  } else {
    toSort = radixSort_LSD(toSort, digit-1);
    return LSDcountingSortOnDigit(toSort, digit);
  }
}
{% endhighlight %}

#### Most Significant Digit Radix Sort

For the Most Significant Digit Radix Sort, although the idea is similar to the Least Significant Digit Radix Sort, however there are some underlying complexities.

The recursive algorithm is as follows:

{% highlight none %}
int[] toSort;
sort toSort on k-th digit:
  if k==0:
    return sort toSort on 0-th digit;
  else:
    sort toSort on k-th array into individual arrays
    for each array:
      sort toSort on (k-1)-th array into individual arrays
    return concat (all arrays)
{% endhighlight %}

After sorting on a single digit, we need to recursively sort each of these groups on the next digit. If we implement the same method as the Least Significant Digit Radix Sort where we sort on a single digit and sort the entire array on the next most significant digit, the end results will only be a jumbled array where the array is sorted on the least significant digit. Hence there is a need to sort each group by themselves and concatenate the results together.

Therefore, the sorting on a single digit for the Most Signicant Radix Sort is slightly different such that the results should not be combined into a single array. In this way, we can apply the sort on the next most significant digit on the individual arrays.

{% highlight java %}
private int[][] MSDcountingSortOnDigit(int[] toSort, int digit){
  int[] count = new int[10];
  int[][] tempArray = new int[10][];
  // determine how big each bucket should be.
  for (int i : toSort){
    count[getNthDigit(i, 10, digit+1)]++;
  }
  for (int i=0 ; i<10 ; i++){
    tempArray[i] = new int[count[i]];
  }
  // input each entry into a single array at the ending index 
  // of its bucket and minus the ending index by 1
  for (int i=toSort.length-1 ; i>=0 ; i--){
    int temp_digit = getNthDigit(toSort[i], 10, digit+1);
    tempArray[temp_digit][--count[temp_digit]] = toSort[i];
  }
  return tempArray;
}
{% endhighlight %}

As with the case of the Least Significant Radix Sort, after implementing the single digit counting sort, the rest is straight forward. The Most Significant Radix Sort is a forward recursion.

{% highlight java %}
public int[] radixSort_MSD(int[] toSort){
  int max = toSort[0];
  for (int i : toSort){
    if (max < i){
      max = i;
    }
  }
  return radixSort_MSD(toSort, String.valueOf(max).length()-1);
}
private int[] radixSort_MSD(int[] toSort, int digit){
  if (digit == 0){
    int[][] temp_array = MSDcountingSortOnDigit(toSort, digit);
    for ( int i=1 ; i<temp_array.length ; i++ ){
      temp_array[0] = concat(temp_array[0], temp_array[i]);
    }
    return temp_array[0];
  } else {
    int[][] temp_array = MSDcountingSortOnDigit(toSort, digit);
    for ( int i=0 ; i<temp_array.length ; i++){
      temp_array[i] = radixSort_MSD(temp_array[i], digit-1);
    }
    for ( int i=1 ; i<temp_array.length ; i++ ){
      temp_array[0] = concat(temp_array[0], temp_array[i]);
    }
    return temp_array[0];
  }
}
{% endhighlight %}

Radix Sort is a expanded version of the counting sort or bucket sort, where the sort is iterated over multiple digits. Radix Sort is also a non-comparison sorting methodology, however unlike counting sort and bucket sort, there should be a fix amount of keys across all digits. For example in decimal, in every digit the number can only be from 0 to 1. Another example is in alphabetical words, in every digit the letters can only be from a-z and A-Z. 

The Most Significant Digit Radix Sort can be used to sort keys into lexicographic order and that unlike the Least Significant Digit Radix Sort, it does not necessarily preserve the original order of duplicate keys. 

Radix Sort takes O(wn) time to run, where w is the number of digits and n is the number of elements. In both the Most Significant and Least Significant Digit Radix Sort, we will eventually go through all elements at each digit. 

## Conclusion

The 2 sorts mentioned above generally runs faster than the comparison sorts that runs on O(n log n) time. Counting sort runs at O(n+k) time and Radix sort runs at O(wn) time. These sorts are seemingly simple to implement but proves to need some brain power during implementation. The source code of my implementation on these linear sorting methods can be found [HERE][1]. It have the basic sorting methods done, but more can be done!

One interesting thing about Radix sort is that it is originally invented to sort punch cards! 

## References

1. <http://www.cs.cmu.edu/~ab/Desktop/15-211%20Archive/res00055/lect06.pdf>
2. <https://en.wikipedia.org/wiki/Counting_sort>
3. <https://en.wikipedia.org/wiki/Radix_sort>

[1]:{{ site.baseurl }}/downloads/2016-12-3-LinearTimeSort.java
