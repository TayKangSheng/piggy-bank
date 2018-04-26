---
layout: post
title: Asymptotic Complexity
published: true
categories: Programming
---

In programmming, we often want to analyse the speed or running time of a certain algorithm that we have implemented or are studying. As the time taken to run an algorithm is heavily dependent on the environmental variables such as the hardwares the machine is equiped with and the environment the algorithm is implemented, Asymptotic Complexity provides a method to compare the running time of algorithms relative to each other by analysing the number of steps taken to complete code. 

## How do we describe the speed of algorithms?

In Asymptotic Complexity, we have 3 general approach that we use to describe an algorithm, the **worse case**, **best case** and **equivilant case**. We want to be able to put the algorithms side by side and be able to have a method of comparing their running time without considering the limitations of environmental variables. More often than not, we compare the worse case scenario because we always want to make sure our algorithm runs in acceptable time in all scenario. 

#### Expressing algorithms in terms of functions.

To describe the complexity of an algorithm, we first have to express it in terms of a mathematical function. For example, given a list of numbers, we want to count the number of times the number '1' appears. An simple implementation of such a function is using a single Forloop as shown below. 

{% highlight python%}
def countOnes(array):
  count = 0
    for i in array:
      if ( i == 1 ):
        count++;
  return count;
{% endhighlight %}

Breaking down the number of steps taken to execute such an algorithm; considering the worse case scenario such that the entire array is made up of '1'. The total number of steps taken to complete the algorithm is "**1 + 2n + 1**" number of steps.

{% highlight python%}
def countOnes(array): 
  count = 0 #1 step
    for i in array: #n (size of array) steps
      if ( i == 1 ): #for every n step, 1 step
        count++; #for every check if true, 1 step
  return count; #1 step to return results
{% endhighlight %}


#### As the value of n tends to infinity

To fairly judge the running time across algorithms, we always consider the limit of the number of steps when the input size tends to infinity. Using the previous example, as the function n tends to inifity, "**1 + 2n + 1**" tends to "**n**". Therefore we say that the algorithm runs in "**n**" time. A general rule of thumb, is to always express the function in its simplest form, keeping only the dominant term.

#### Types of functions  

{% highlight none %}
Linear Function - an + b  
Quadratic Function - an^2 + bn + c  
Polynomial Function - an^z + . . . + an^2 + a*n^1 + a*n^0, 
	where z is some constant  
Exponential Function - a^n, where a is some constant  
Logarithmic Function - log n  
Constant - a, where a is come constant  
{% endhighlight %}

To express these in their simplest form, keeping in mind the results when **n** tends to infinity, we only keep the dominant term.

{% highlight none %}
Linear Function - n  
Quadratic Function - n^2  
Polynomial Function - n^z, where z is some constant
Exponential Function - a^n, where a is some constant  
Logarithmic Function - log n  
Constant - 1
{% endhighlight %}

## Asymptotic Notations

Asymptotic Notations are symbols used to represents the 3 cases that we want to consider, the **worse case**, **best case** and **equivalent case** or to compare the running speed of functions.

### The O notation

The **O** notation is used to describe the worse case senario, which really means, the upper bound of the running time of the function. 

#### Big-O 
**Big-O** is used to describe the asymptotic upper bound of the increase/growth of the function. The exact definition of Big-O is as follows:

{% highlight none %}
f(n) = O(g(n)) ⇔ ∃M>0, n0 
	s.t. |f(n)|≤M|g(n)| for n≥n0
{% endhighlight %}

In layman terms, f(n) is equals to O(g(n)) means that there exist a value M bigger than 0 and n0 such that modulus of f(n) is bigger or equals to M multiplies by modulus of g(n) for any value of n bigger than n0.

Going back to the example above:
{% highlight none %}
countOnes(n) = 2n + 2
{% endhighlight %}

Therefore, we say the complexity of the countOnes(n) = O(n).

How do we use the Big-O to compare different functions? Given the following example,
{% highlight none %}
f(n) = 3log n + 100
g(n) = log n
{% endhighlight %}

If we say that the complexity of f(n) is O(g(n)), it means that there exist a value of M such that Mod(f(n)) <= M Mod(g(n)) for any value of n bigger or equals to n0.

{% highlight none %}
f(n) <= M g(n)
3log n + 100 <= M * log n
{% endhighlight %}

True enough, the above expression is true when n>2, because f(n) and g(n) is undefined when n=1. Therefore the definition of **Big-O** is fulfilled and we can say that f(n) is O(g(n)).

#### Small-o

**Small-o** is simply the same as **Big-O** without any asymptotic upper bound. In another words, f(n) is o(g(n)) if there exist a value M such that f(n) >= M g(n) for all values of n.

### The Omega notation

The Omega notation is used to describe the best case senario, which really means, the lower bound of the running time of the function. 

#### Big-Omega (Ω)

**Ω** is used to describe the asymptotic lower bound of the increase/growth of the function. The exact definition of Ω is as follows:

{% highlight none %}
f(n) = Ω(g(n)) ⇔ ∃M >0, n0 
	s.t. |f(n)|≥M|g(n)| for n≥n0
{% endhighlight %}

In layman terms, f(n) is equals to Ω(g(n)) means that there exist a value M bigger than 0 and n0 such that modulus of f(n) is smaller or equals to M multiplies by modulus of g(n) for any value of n bigger than n0.

Going back to the example above:
{% highlight none %}
countOnes(n) = 2n + 2
{% endhighlight %}

Therefore, we say the complexity of the countOnes(n) is also Ω(n). Because the Forloop does not contain any **break** method and the Forloop has to be completed eventually.

#### Small-Omega (ω)

**Small-Omega (ω)** is simply the same as **Big-Omega (Ω)** without any asymptotic upper bound. In another words, f(n) is ω(g(n)) if there exist a value M such that f(n) >= M g(n) for all values of n. 

### Theta Θ

**Theta Θ** is used to describe the equivalent senario. The exact definition is as follows:

{% highlight none %}
f(n) = Θ(g(n)) ⇔ ∃M1,Μ2 > 0, n0 
	s.t. M1g(n) ≤ f(n) ≤ M2g(n) for n≥n0
{% endhighlight %}

In layman terms, f(n) is equals to Θ(g(n)) means that there exist a value M1 and M2 bigger than 0 and n0 such that M1g(n) is smaller or equals to f(n) and M2g(n) is bigger than f(n) for any value of n bigger than n0.

Therefore f(n) is both O(g(n)) and Ω(g(n)).

## Conclusion

**Big-O** is primarily the notation used to analyse algorithm time complexity. The rest of the notations can come in helpful in different situations especially so during an Technical Interview where interviewers just want to test you on how much you know.  

Note that this concept can be applied to different mathematical expressions not confined to the field of computer science. These symbols are used in Complexity Theory, Computer Science and Mathematics to describe asymptotic behaviour of functions. Below are some readings and reference that i have found helpful. Dive into them for more proves and examples on this topic if you are keen on learning more.  

# References

1. https://learnxinyminutes.com/docs/asymptotic-notation/
2. Singapore University of Technology and Design, Information Systems Technology & Design, 50.004 Intro to Algorithms Slides
3. http://web.mit.edu/16.070/www/lecture/big_o.pdf









