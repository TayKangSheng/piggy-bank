---
layout: post
title: A Heap Data Structure
published: true
categories: Programming
---

In computer science, a heap is a specialized tree-based data structure that satisfies the heap property: If A is a parent node of B then the key (the value) of node A is ordered with respect to the key of node B with the same ordering applying across the heap. [1]

**A Heap is basically an implementation of a [priority queue](https://en.wikipedia.org/wiki/Priority_queue)**, therefore before we jump into how does a heap work and what is it for, we should first understand what is a **priority queue**.

## What is a Priority Queue?

Priority Queue is an Abstract Data Type (ADT) implementing a set S of elements (each associated with a key) supporting the following operations.  
k(x) = key of element x

1. max(S): returns element from S with largest key
2. insert(S,x): insert element x into set S
3. extract_max(S): return element from S with largest key and remove it from S
4. increase_key(S,x,k'): increase the value of element x's key k(x) to new value k' (assumed to be larger than the current value) 

#### Example of a priority queue

A basic priority queue can be implemented using a array to store the elements. Note that the definition above clearly states that it is a set, which means that there can be no duplicate keys in the same set. Therefore in the case of a array, we do not allow for duplicate elements. Of course changes can be made at your own discretion if allowing for duplicate is something that will work for you. 

{% highlight java %}
public class PriorityQueue {
  private ArrayList<Integer> elements;
  
  public PriorityQueue(){
    elements = new ArrayList<Integer>();
  }

  public int Max(){
    int largest_element = elements.get(0);
    for (int i=1; i<elements.size(); i++){
      if (elements.get(i) > largest_element ){
        largest_element = elements.get(i);
      }
    }
    return largest_element;
  }
	
  public boolean insert(int x){
    if (!elements.contains(x)){
      elements.add(x);
      return true;
    }
    return false;
  }
	
  public int extract_max(){
    int max_element = this.Max();
    elements.remove(max_element);
    return max_element;
  }
	
  public  boolean increase_key(int key, int value){
    for (int i=0 ; i<elements.size() ; i++){
      if (elements.get(i) == key){
        elements.set(i, elements.get(i)+value);
        return true;
      }
    }
    return false;
  }
}
{% endhighlight %}

## Heap and how it looks like.

As mentioned before, a heap is an implementation of a priority queue. It is an array visualised as a nearly complete binary tree. Navigating on the binary tree is actually just moving along an array of elements. 

![Heap]({{ site.baseurl }}/images/2016-11-26-1.png){: .center-image }

There are 2 main types of Heap, the Max Heap and the Min Heap. In a Max Heap, the keys(the value) of parent nodes are always greater or equals than those of the children nodes and the largest key is the root node. In a Min Heap, its the other way around, where the parent nodes are always smaller or equals than those of the children nodes, and the smallest key is the root node.

How do we visualise an array as a tree? 

1. The root of the tree corresponds to the **index = 1** of the array. 
2. The index of the parent node of node at index i is (i/2). e.g. parent(5)=2.
3. The index of the left child of node at index i is 2(i). left(4)=8.
4. The index of the right child of node at index i is 2(i)+1. right(4)=9.
5. NOTE: The first index must be 1, if not the mathematical rules to find the children nodes will not work.

Some basic properties of a Heap.

1. There is no need for pointers as the the parent and child nodes can be found using simple mathematical operations.
2. The height of a binary Heap is O(log n), which means at most log n. Therefore any path from root to the leaf node is at most log n steps. 

Operations with Heap (max).

1. build_max_heap: Produce a max-heap from an unordered array in O(n).
2. max_heapify: correct a single violation of the heap property occuring at the root of a subtree in O(log n).
3. insert: Insert a node into the tree and re-order if needed to not violate the heap property.
4. extract_max: Extract the root node and re-order the tree such that it does not violate the heap property.
5. heapsort: sort an array of size n in O(n log n) using heaps.

#### max_heapify

The point of this operation is to correct a violation of the heap property occuring at the root of a subtree. If either key of the left-child or the right-child of this root node is bigger than the key of this root node, the property of the Max Heap is violated. Therefore we want to solve this violation by trickling this key down the tree until there is no more violation of the Heap property. Below is a graphical representation of what we want to achieve in this operation. 

![max_heaify]({{ site.baseurl }}/images/2016-11-26-2.png){: .center-image }

Therefore, there are 2 main steps in this operations:

1. Find the index of the largest key among A[i], A[left(i)] and A[right[i]]
2. If this index is different than i, exchange A[i] with largest key; then recurse on subtree with the root node of the recursed subtree as the previous A[i] key. This is to ensure that after the key is trickled down, we want to check recursively until there is no more violations of the Heap property.

{% highlight java %}
public void max_heapify(int index){
  int left_index = (index*2);
  int right_index = (index*2)+1;
  int largest_index;
  if (left_index < heap.size() && heap.get(left_index)>heap.get(index) ){
    largest_index = left_index;
  } else {
    largest_index = index;
  }
  
  if (right_index < heap.size() && heap.get(right_index)>heap.get(largest_index) ){
    largest_index = right_index;
  }
  
  if (largest_index != index){
    int temp = heap.get(largest_index);
    heap.set(largest_index, heap.get(index));
    heap.set(index, temp);
    max_heapify(largest_index);
  }
}
{% endhighlight %}

#### build_max_heap

If given an array of keys, we want to convert this array into a Max Heap. This operation can be executed using the **max_heapify** operation we have implemented above. One observation we can make is that while arranging the keys in the Heap, violation can only occur at nodes 1..[n/2] as the nodes [n/2]+1..n are leaf nodes. 

Therefore this operation only consist of 1 step:

1. Initialise all elements of the given array into an unsorted array. 
2. Loop backwards from node n/2 to the root node 1, and do max_heapify.

{% highlight java %}
public void build_max_heap(int[] array){
  heap.add(null); // This is to ensure that the index of the first element is 1;
  for (int i=0 ; i<array.length ; i++){
    heap.add(array[i]);
  }
  for (int i=heap.size()/2; i>0 ; i--){
    max_heapify(i);
  }
}
{% endhighlight %}

#### insert

How do we insert an node in a heap yet not violating any heap property? The general idea is to insert the node at the end of the array, then recursively check whether the key of the parent node is bigger or smaller than the new node. If yes, switch the position of the parent node and the new node, then check the key of the new parent node of the new node. So we slowly trickle the node from the bottom up until it finds a place where it does not violate the Max_Heap property.

{% highlight java %}
public void insert(int value){
  heap.add(value);
  int current_index = heap.size()-1;
  while (current_index>1){
    int parent_index = current_index/2;
    if ( heap.get(parent_index) < heap.get(current_index) ){
      int temp = heap.get(parent_index);
      heap.set(parent_index, heap.get(current_index));
      heap.set(current_index, temp);
      current_index = parent_index;
    } else {
      break;
    }
  }
}
{% endhighlight %} 

#### extract_max

Now that we have implemented the **max_heapify** operation, we can use the **max_heapify** operation to extract the node with the largest key in the Heap. The idea is simple. First switch the position of the root node and the last node in the array. Now remove the original root node, and use **max_heapify** on the new root node. As the new root node is originally a leaf node, it will now trickle down again to the base while re-arranging the Heap into a new position which does not violate the Max Heap property.

{% highlight java %}
public int extract_max(){
  if (heap.size() <= 2){
    return heap.remove(1);
  } else {
    int max = heap.get(1);
    int last_node = heap.get(heap.size()-1);
    heap.remove(heap.size()-1);
    heap.set(1, last_node);
    max_heapify(1);
    return max;
  }
}
{% endhighlight %}

#### heapsort

**Heapsort** is the last operation that we are going to discuss in this article. As mentioned above **Heapsort** is a sorting algorithm that sorts an array of size n in O(n log n) using Heaps.

Now that we have gone through the various operations of a Heap, implementing Heap sort is now trivial. We can simple call the **extract_max** operation n times to produce a sorted array. 

{% highlight java %}
public int[] heapsort(){
  int[] sorted_array = new int[heap.size()-1];
  for (int i=0 ; i<sorted_array.length ; i++){
    sorted_array[i] = extract_max();
  }
  return sorted_array;
}
{% endhighlight %}

## Time Complexity

Now that we have gone through so much about how to implement a Heap data type, how efficient is actually the Heap is manipulating data.

||| **Time Complexity** || **Operation** | 
|:--|--|:----------------|-|:----------|
|a) || O(n) || build_max_heap |
|b) || O(log n) || max_heapify |
|c) || O(log n) || insert, extract_max, increase_key | 
|d) || O(n log n) || heapsort |

Keep in mind that the height of the **Heap** is at most log n, therefore searching for a key is at most log n steps. As with operations such as **max_heapify**, if the node that needs to be checked is right at the top, the worse case senario is to move the node all the way node to the bottom which is at most log n steps.  

## Conclusion

**Heaps** can be a simple and effective data structure. It might not be the fastest data structure to access your data, but it is simple to implement and works fairly effectively. The codes that i have shown above can be downloaded [HERE][1]! If you have found that the code is wrong in anyway, feel free to drop me an email and we can discuss how to make the code better. It is a never ending learning process as a programmer and I am too forever a student of the Computer Science domain. 


# References
1. https://en.wikipedia.org/wiki/Heap_(data_structure)
2. Singapore University of Technology and Design, Information Systems Technology & Design, 50.004 Intro to Algorithms Slides

[1]:{{ site.baseurl }}/downloads/2016-11-26-MaxHeap.java

















