---
layout: post
title: Difference between an Array and List?
published: true
categories: Programming
---

Recently I was asked a supposedly basic question that really is at the core of programming, and that is "What is the difference between an Array and a List?". The idea of Array and List is so basic that i have never actually gone deep into the difference between the 2; but instead use them loosely treating them as the same. Therefore i was dumbfounded when i was posted this question, especially during an important technical interview and i was pretty disappointed with myself for not being keen enough to pick on the basic definitions while dreaming of building large applications. So i hope this post serves as a reminder of our basic knowledge, and that it helps you as much as it is going to help me grow as a computer programmer.

Before jumping into the difference between the Array and List, we first have to understand what is the difference between **data types**, **abstract data type (ADT)** and **data structure**. I will tell you the answer first, so that you can keep them in context while trying to understand the differences between data types, ADT and data structure. An Array is a **Data Type** and a **List** is both an ADT and Data Structure.

## Data Type and Why is Array a Data Type?

A Data Type, or sometimes refered to as Primitive Data Type are only single values without any special capabilities. Examples of Data Types are "int", "double", "float","char", "boolean" etc. These Data Types usually have a particular declared size in number of bits at the logic level. For example a integer is 32bits long and a double is 64bits long. Declaring a data type when intialising a variable instructs the compiler to set aside a portion of memory large enough to hold the data of that type. 

{% highlight java %}
int i; 
// compiler sets aside 32bits of memory for variable i
double length; 
// compiler sets aside 64bits of memory for variable length
{% endhighlight %}

Why then is there a need for data types? Because given the same number of bits of data, the bits can be intepreted vastly differently, for example, a simple 8bit '01000010' can be intepreted as the character 'B', the value 9.2E-44 in double or float, or simply the number 66 as integer.

So then, why is **Array** a Data Type? An **Array** is basically a collection of homogenous items which are arranged contiguously. Therefore all elements in an **Array** must be of the same data type and that it is placed one after another sequentially in memory. Hence in memory, if element 1 is placed in a particular location, element 2 will be placed right after it, and repeatedly for the rest of the elements. An example of an array in Java is as follows:

{% highlight java %}
int[] array = new int[10];
// compiler sets aside (32*10)bits of memory for variable array
{% endhighlight %}

If you have tried it for yourself, if you declare the array without stating the size of the array, your code will not be allowed to be compiled. Then compare the array data type to the primitive data types mentioned above, the only difference between them is that an array is a collection of the data types ordered squentially in memory. 

## List as a Abstract Data Type (ADT) and Data Structure.

To understand the difference between ADT and Data Structure, i found a link online where the author aptly divides the 2 terms. I have put the link under **References** below if you want to read up more about the difference between the 2.

> To put it simple, ADT is a logical description and data structure is concrete. ADT is the logical picture of the data and the operations to manipulate the component elements of the data. Data structure is the actual representation of the data during the implementation and the algorithms to manipulate the data elements. ADT is in the logical level and data structure is in the implementation level.

A **Abstract Data Type** (ADT) is simple a logical description of what is this data type supposed to do and what are the operations that can be done with it. An example of a ADT, is a **[Priority Queue](https://en.wikipedia.org/wiki/Priority_queue)**. The logical description of a **Priority Queue** is that it is a queue where each element has a "priority" associated with it. In this queue, an element with higher priority is served first before elements with lower priority is served and that if 2 elements have the same priority, they are served according to their order in the queue. The 2 operations that this ADT must support are **insert_with_priority** and **pull_highest_priority**. However, this is only a logical description without specifying how exactly it should be implemented and only provides an overview of what it should be and what it should do.

On the other hand, a **Data Structure** is an implementation of a way data is organised. By data, it can be of the same or different **Data Type** purely decided by how the Data Structure is implemented. In many cases such as in OOP, the details of how the **Data Structure** is implemented is unimportant unless when efficiency is an issue. In this case, a **List**, although logically similar to an **Array** to provide a sequential method for users to acess and manipulate elements might not be as straight forward as setting aside memory space contiguously. In fact, most often than not, a List data structure provides more functions and sometimes a more ingenious way to arrange data to improve access time. An example of a List Data Structure is as below:

{% highlight java %}
List<Integer> list = new List<Integer>();
{% endhighlight %}

So when is a List a ADT and when is it a Data Structure? Simply put, a **List** is a ADT when discussed using words and a **List** is a data structure when it is implemented in code for you to use. Each programming language usually have their own implementation of List such as [Python](https://www.tutorialspoint.com/python/python_lists.htm) and [Java](http://docs.oracle.com/javase/8/docs/api/java/util/List.html). 

## Differences between an Array List? 

Now the question that i was dumbfounded by. The key difference between an Array and List is that an Array is a Data Type and a List is a Data Structure. That being said, an Array must declare its size at right at the start so that the correct amount of memory space can be allocated. But instead the List, more often than not, does not require the user to declare the size at the start. This is why most of the time List is more versatile to deal with than an array. Understand that a List is a data structure and an Array is a Data Type; then you will be able to understand all the differences between List and Array. 

{% highlight java %}
// This is an array
int[] array = new int[20];
// These are Lists
List<Integer> list = new List<Integer>();
ArrayList<Integer> arrayList = new ArrayList<Integer>();
{% endhighlight %}

## Conclusion

Last but not least, i would like to say, knowing the definitions of the terms is important for you to keep yourself clear of what you would like to express. However, different programming languages might refer to them differently and it is of some good to express some flexibility when using the terms. For example if you open the link on Python about List above, it says that a List is a Data Type. Why so? Because different programming languages have a different set of what they refer to as **Data Type**. So when communicating in these programming languages, know the jargons and try not to get yourself mixed up. All in all, understanding is key! Why is there a need to split the seemingly similar **Array** and **List**.

## Reference
1. <http://www.cs.cmu.edu/~mrmiller/15-110/Handouts/primitiveData.pdf>
2. <https://en.wikipedia.org/wiki/Array_data_type>
3. <https://www.quora.com/What-is-the-difference-between-an-array-a-list-and-a-linked-list>
4. <https://abrickshort.wordpress.com/2005/03/06/abstract-data-types-vs-data-structures/>
5. <https://en.wikipedia.org/wiki/Priority_queue>
