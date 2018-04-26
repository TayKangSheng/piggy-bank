---
layout: post
title: Binary Search Trees
published: true
categories: Programming
---

In this article we are going to examine what are Binary Search Trees (BST), how to implement it and  the performance of BST as a Data Structure.

## Binary Tree

So before we start on BST, what is a Binary Tree? Binary Binary trees are simply a data structure made of nodes where each node have a Left child node, a Right child node and a Data element. The topmost node is called the root node. 

Each node (excluding the parent node) is connected by a direct edge from another node called its Parent node; and each node can be connected by a direct edge to either 0, 1 or 2 nodes called Children nodes. If a node does not have any children nodes, it is called a leaf node or external node. Else, it is called an Internal node. Nodes with the same parents are called siblings. 

## Binary Search Tree

Binary Search Trees (BST) are actually a particular kind of Binary Trees. The main motivation of BST is to provide a way to efficiently sort, search and retrieve data from a set of data. 

The charateristics of a BST is as follows:

1. Each node contains one key (data element)
2. The keys on the left subtree are less than the key in its parent node.
3. The keys on the right subtree are more than the key in its parent node.
4. Duplicate keys are not allowed.

As the Left child node and right child node of a parent node are also subtrees by themselves, the above characteristic is recursively appiled to all nodes in the BST tree. An example of a BST is as below: 

![BST]({{ site.baseurl }}/images/2016-11-28-BST.bmp){: .center-image }

Some of the common operations a BST should be able to support are: **Insert**, **Search**, **FindMax**, **FindMin**, **Delete**.

### Implementation

*Disclaimer: This implementation is heavily based on this [article](https://www.cs.cmu.edu/~adamchik/15-121/lectures/Trees/trees.html) and you can find the full implementation in the article too. What i have in this article instead, is a built on and my learnings from understanding the explanations and the code in that article.*

This implementation that we are going to go through in this article is a BST with a Generic Data Type. This is so to have a BST that is capable of handling object of any type. A BST class can be implemented with a structure such as below, with a private inner class Node. Note that because of the nature of BST, there is a need to be able to compare the Data Type and therefore in this implementation, the Generic Data Type extends the Java class Comparable. 

{% highlight Java %}
public class BST<T extends Comparable<T>>{
  // Variables
  ...
  // Constructors
  ...
  // Helper Methods
  ...
  // Methods
  ...
  private class Node<T>{
    private T data;
    private Node<T> left, right;
    public Node(T value){
      data = value;
      left = right = null;
    }
    public String toString(){
      return data.toString();
    }
  }
}
{% endhighlight %}

Due to the recursive nature of the data structure, all operations always have to start from the root node. Therefore we only need to save one node and that is the root node. Operations such as Insert, Search and etc all have to start from the root node in order to get to the correct position on the tree. Hence, the inner class Node does not need to save a reference link to its Parent node either. Another variable we have saved is the Comparator, because we want users to use this BST for any DataType and therefore this Comparator objects provides a way for users to be able to compare the DataType properly.

#### Constructors

Constructors are the key method to initalise a class. As we have provided a Generic DataType for the BST class, we want to be able to make proper comparison for any DataType provided when using this BST class. Therefore we provide 2 ways of initialising this BST tree, one is without custom Comparator and the other with a custom Comparator.

{% highlight Java %}
// Constructors
public BST(){
  root = null;
  comparator = null;
}
public BST(Comparator<T> comp1){
  root = null;
  comparator = comp1;
}
{% endhighlight %}

#### Helper Methods

As mentioned above, we need a way to compare the DataType. If the DataType is a primitive numerical DataType such as int and float, there is no need to implement another comparator. However if the DataType is otherwise, then we have to properly implement a helper method to allow us to compare the value of the DataType.

{% highlight Java %} 
private int compare(T x, T y) {
  if (comparator == null) {
    return x.compareTo(y);
  } else {
    return comparator.compare(x, y);
  }
}
{% endhighlight %}

#### Insert

The Insert method employs a recursive search down the tree from the root node. Knowing the rules governing the data structure, we can simply traverse down the tree, going left if the key to be inserted is smaller than the current node, and going right if otherwise. If at any point in time, the key is found to already exist in the node, we simply stop the Insert; because duplicate keys are not allowed in a BST. Therefore a new Insert always replaces a null reference.

{% highlight java %}
// A public and a private method is to provide
// encapsulation of the recursive insert method.
public void insert(T toInsert) {
  root = insert(root, toInsert);
}

private Node<T> insert(Node<T> parent, T toInsert) {
  if (parent == null) {
    // We have reached the end of the tree and found
    // the correct position to insert the new key.
    parent = new Node<T>(toInsert);
  } else {
    // First check whether the toInsert key is the
    // same as the key of the current Node.
    if (compare(toInsert, parent.data) == 0) {
      return parent;
    }
    // Check whether the toInsert key is bigger or equal
    // to the current key. If smaller, insert in the left
    // subtree. Else, insert in the right subtree.
    if (compare(toInsert, parent.data) < 0) {
      parent.left = insert(parent.left, toInsert);
    } else {
      parent.right = insert(parent.right, toInsert);
    }
  }
  return parent;
}
{% endhighlight %}

#### Search

Searching through a BST always start from the root node. From the root node, if the key we are searching for does not match the key of the root node, we compare the value of the key with the key of the root node. If the value of the key is smaller, we search for the key in the left subtree, otherwise we search for the key in the right subtree. From here, we recursively search down the tree until we reach the end of the tree which means the key does not exist, or we find an match somewhere along the path.

{% highlight Java %}
// Again we first start with Encapsulation
public boolean search(T toSearch){
  return search(root, toSearch);
}
private boolean search(Node<T> parent, T toSearch){
  if ( parent == null ){
    // We have come to the end of the tree. 
    // Key has not been found.
    return false;
  }
  if ( compare( toSearch, parent.data ) == 0 ){
    // We have found a match with the
    // key we are looking for!
    return true;
  }
  // If key is smaller than parent.data, 
  // search in left subtree. Else search in
  // right subtree.
  if ( compare( toSearch, parent.data ) < 0 ){
    return search(parent.left, toSearch);
  } else {
    return search(parent.right, toSearch);
  }
}
{% endhighlight %}

#### FindMax & FindMin

Keeping the simple rule of BST in mind that smaller keys always go to the left of the tree and larger keys always go to the right of the tree. The find max and min methods can be implemented in just a few lines of code.

{% highlight java %}
// Again, encapsulation first.
public Node<T> findMax(){
  return findMax(root);
}

private Node<T> findMax(Node<T> parent){
  while (parent != null){
    // Keep going until the furthest right of the tree
    parent = parent.right;
  }
  return parent;
}
{% endhighlight %}
{% highlight java %}
// Again, encapsulation first
public Node<T> findMin(){
  return findMin(root);
}
private Node<T> findMin(Node<T> parent){
  while (parent != null){
    // Keep going until the furthest left of the tree
    parent = parent.left;
  }
  return parent;
}
{% endhighlight %}

#### Delete

Deleting is a bit more tricky than the previous 2 methods. Removing a node means that in some cases, we have to edit the links between nodes such that subtrees will not be un-reachable after deleting a certain node. There are a total of 4 cases when we want to delete a single node: 

1. Key is not in the tree
2. Node is a leaf node
3. Node has only 1 child
4. Node has 2 children.

If the key is not in the tree, there is nothing to delete. If the key is in a leaf node, we can simply make this node become a null reference. If this node has only 1 child, then like a link list, we can update this node to become the child node thus bypassing the node that is to be deleted. 

![Delete1]({{ site.baseurl }}/images/2016-11-28-Delete1.bmp){: .center-image }

The trickiest is when you want to delete a node that has 2 child nodes. If we delete such a node, we have 2 separate subtrees below it and we have to find a way to make this 2 subtrees into a single tree to replace the deleted node. Therefore the strategy is the following: *replace the node being deleted with the largest node in the left subtree and then delete that largest node. By symmetry, the node being deleted can also be swapped with the smallest node is the right subtree, and then delete that smallest node.*

![Delete2]({{ site.baseurl }}/images/2016-11-28-Delete2.bmp){: .center-image }

{% highlight java %}
// Again always remember to encapsulate functions.
public void delete(T toDelete){
  root = delete(root, toDelete);
}
private Node<T> delete(Node<T> parent, T toDelete){
  if ( parent == null ) {
    throw new RuntimeException("Cannot Delete.");
  }
  if ( compare( toDelete, parent.data ) < 0 ){
    parent.left = delete( parent.left, toDelete );
  } else if ( compare( toDelete, parent.data) > 0 ){
    parent.right = delete( parent.right, toDelete );
  } else {
    if ( parent.left == null ){
      parent = parent.right;
    } else if ( parent.right == null ){
      parent = parent.left;
    } else {
      parent = findMax(parent.left);
      parent.left = delete(parent.left, parent.data);
    }
  }
  return parent;
}
{% endhighlight %}

### Tree Traversal

One key characteristic of a BST is that you can output the nodes with keys in a ascending order using In-order traversal. Traversal is a process that visits all the nodes in the tree. Since a tree is not a linear data structure, we can visit all the nodes in the tree in 2 approaches. The first is depth first traversal and the second is breath first traversal. 

There are 3 different kinds of depth first traversal:

1. Pre-Order Traversal - visit the parent first and then left and right children.
2. In-Order Traversal - visit the left child first, then the parent and right child.
3. Post-Order Traversal - visit the left child, then the right child and then the parent.

![TreeTraversal]({{ site.baseurl }}/images/2016-11-28-TreeTraversal.png){: .center-image }

The black dots on the nodes are the order where the nodes are visited. These traversal can be represented in a single algorithm by assuming that we visit each node 3 times. An **Euler tour** is a walk around the binary tree where edges are treated as walls, which you cannot cross. During the walk around the tree, you will visit the nodes 3 times each. Following the left wall of each node, you will be visiting the nodes in the Pre-Order Traversal. Following the bottom wall of each node, you will be visiting the nodes in the In-Order Traversal. Following the right wall of each node, you will be visiting the nodes in the Post-Order Traversal.

![TreeTraversal]({{ site.baseurl }}/images/2016-11-28-TraversalEuler.bmp){: .center-image }

As for breath first traversal, there is only one kind which is the level order traversal. Which means that you visit the nodes level by level from the root node. 

#### Implementation

Implementation of the 3 depth first traversal can be done recursively and is very similar. For Pre-Order traversal, we will print the key of the node the moment the node is visited for the first time. 

{% highlight java %}
//Again, Encapsulation First!!
public void preOrderTraversal(){
  preOrderTraversal(root);
  System.out.println();
}
private void preOrderTraversal(Node<T> parent){
  if (parent != null){
    System.out.print(parent);
    preOrderTraversal(parent.left);
    preOrderTraversal(parent.right);
  }
}
{% endhighlight %}

For In-Order traversal, we will print the key of the node the second time the node is visited.

{% highlight java %}
//Again, Encapsulation First!!
public void InOrderTraversal(){
  InOrderTraversal(root);
  System.out.println();
}
private void InOrderTraversal(Node<T> parent){
  if (parent != null){
    InOrderTraversal(parent.left);
    System.out.print(parent);
    InOrderTraversal(parent.right);
  }
}
{% endhighlight %}

Last but not least, for Post-Order traversal, we will print the key of the node the last time the node is visited.

{% highlight java %}
//Again, Encapsulation First!!
public void PostOrderTraversal(){
  PostOrderTraversal(root);
  System.out.println();
}
private void PostOrderTraversal(Node<T> parent){
  if (parent != null){
    PostOrderTraversal(parent.left);
    PostOrderTraversal(parent.right);
    System.out.print(parent);
  }
}
{% endhighlight %}

### Iterator

After implementing a BST object. What if we want to iterator through the nodes of the BST sequentially? An example below is something that we as programmers always use to quickly iterate through the values of a dataset: 

{% highlight java %}
BST<T> bst = new BST<T>();
...
for (T i : bst){
	...
}
{% endhighlight %}

In this case, we are going to implement the iterator to iterate through the BST in Pre-Order. Therefore the first thing to do is how do we implement an iterator? We start by making BST implement the Iterable interface.

{% highlight java %}
public class BST<T extends Comparable<T>> implements Iterable<T> {
...
}
{% endhighlight %}

After implementing the Iterable interface, if you are using any decent IDE (Integrated Development Environment), it should prompt you to add the unimplemented methods of the Iterable interface; which is the iterator() function. We are going to implement the iterator as a private inner class of the BST class. Follow the "implementing iterable" article in the reference if you want to read up more. 

{% highlight java %}
// This is the method from Iterable interface
// that you will be prompted to implement
@Override
public Iterator<T> iterator() {
  return new BSTiterator();
}
// We are going to implement our own Iterator as a 
// private inner class inside the BST class.
private class BSTiterator implements Iterator<T>{
  Stack<Node<T>> stk = new Stack<Node<T>>();
  public BSTiterator(){
    if (root != null){ stk.push(root); }
  }
  @Override
  public boolean hasNext() {
    return !stk.isEmpty();
  }
  @Override
  public T next() {
  if (stk.isEmpty()) throw new java.util.NoSuchElementException();
    Node<T> cur = stk.pop();
    if (cur.right != null) stk.push(cur.right);
    if (cur.left != null) stk.push(cur.left);
    return cur.data;
  }
}
{% endhighlight %}

The main problem with implementing this iterator interface is that it is easy to generate a whole list of keys by Pre-Order traversal in one shot using recursion, however in this interator class, we have to implement the Pre-Order traversal such that calling the next() method steps through the recursion. In another words, it "requires the implicit recursive stack implemented explicitly". The implementation above seems to work like magic, however there is a need to understand the rational behind it in order to produce it again and again. 

So here is how to step through the Pre-Order Traversal of BST. The algorithm starts with the root and push it on a stack. When a user calls for the next() method, we check if the top element has a left child. If it has a left child, we push that child on a stack and return a parent node. If there is no a left child, we check for a right child. If it has a right child, we push that child on a stack and return a parent node. If there is no right child, we move back up the tree (by popping up elements from a stack) until we find a node with a right child. 

The implementation of the algorithm below looks like magic to me too, but i still manage to step through it using pen and paper. So try it for yourself and understand the magic that goes on inside the brain of Computer Scientist.

{% highlight java %}
public T next()
{
  Node cur = stk.peek();
  if(cur.left != null) {
    stk.push(cur.left);
  } else {
    Node tmp = stk.pop();
    while(tmp.right == null) {
      if (stk.isEmpty()) return cur.data;
      tmp = stk.pop();
    }
    stk.push(tmp.right);
  }
  return cur.data;
}
{% endhighlight %}

### Time Complexity

So how does a BST as a data structure fare? 

Assuming we have a complete binary tree, which means that the tree has the best possible ratio between the number of nodes and the height. Which really means that the nodes is equally distributed at both sides. Then the height of the BST is at most O(log n). Therefore **Insert**, **Search**, **FindMax**, **FindMin**, **Delete** functions are O(log n).

Constructing the BST will take O(n log n). To sort a data set using a complete binary tree means that you have to firstly construct the tree, and use Pre-Order Traversal. Hence is n + n log n = O(n log n).

||| **Time Complexity** || **Operation** | 
|:--|--|:----------------|-|:----------|
|a) || O(log n) || Insert |
|b) || O(log n) || Search |
|c) || O(log n) || FindMax, FindMin | 
|d) || O(log n) || Delete |
|e) || O(n log n) || sort | 

## Conclusion

Trees are an extended concept of linked data structures. Trees exhibits many advantages as Data Structures and therefore they are frequently used. BST is one such simple example of a tree with a small additional rule. Keeping in mind the idea of trees as data structures, rules can be added and remove with flexibility to suit the context of the problem. 

I have learnt alot writing this article and implementing the BST class. My code can be downloaded [HERE][1]. I have heavily referenced the first article in the Reference section below and has treated it as my main learning material for this Article. There is a source code provided in there too, and it is definitely worth a read. Some of the implementations are pretty deep and ingenious so please go and have a look. It is definitely worth learning how to write such awesome code. 

## References

1. <https://www.cs.cmu.edu/~adamchik/15-121/lectures/Trees/trees.html>
2. Singapore University of Technology and Design, Information Systems Technology & Design, 50.004 Intro to Algorithms Slides
3. <http://tutorials.jenkov.com/java-generics/implementing-iterable.html>

[1]:{{ site.baseurl }}/downloads/2016-11-28-BST.java
