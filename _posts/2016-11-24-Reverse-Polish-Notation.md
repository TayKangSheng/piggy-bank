---
layout: post
title: Reverse Polish Notation
published: true
categories: Programming
---

Recently, i was tasked to write a program where input is RPN. Never have i heard of RPN before; naturally the only reasonable thing to do is to ask [Google](https://www.google.com.sg/webhp?hl=en&sa=X&ved=0ahUKEwjWo5iphMHQAhUIPI8KHeDiABwQPAgD#hl=en&q=rpn+programming).  

So according to [Wikipedia](https://en.wikipedia.org/wiki/Reverse_Polish_notation),  

> Reverse Polish notation (RPN) is a mathematical notation in which every operator follows all of its operands, in contrast to Polish notation (PN), which puts the operator before its operands.

Another name for RPN is **Postfix Notation**, where Mathematical expressions do not need any parentheses as long as each operator has the correct number of operands. My personal experience tells me that getting parentheses out of the way actually saves us a whole lot of time trying to parse the expression and getting the expression evaluated in the right order.

Here are some examples:  
{% highlight python%}
Postfix: 5 2 +
Infix: 5 + 2
{% endhighlight %}

{% highlight python%}
Postfix: 3 7 + 2 ×
Infix: (3 + 7) × 2
{% endhighlight %}

{% highlight python%}
Postfix: 5 1 2 + 4 × + 3 −
Infix: 5 + ((1 + 2) × 4) − 3
{% endhighlight %}

So how do you read RPN? The way to read RPN is to go from left to right, and whenever you see an operator, group the operator with the 2 operands or group right before it. If at any point in time, an operator does not have enough operands to work on, the expression is an incoherent expression. In order to evaluate RPN efficently, it is recommended to use a [Stack](https://en.wikipedia.org/wiki/Stack_(abstract_data_type))! If you go to the Wikipedia website above, you will actually find a Postfix Algorithm written in Pseudo code which is fairly straightforward.  

Below is an implementation of a RPN Evaluator written in Java.  

{% highlight java %}
import java.util.Stack;

public class RPNEvaluator {
  public static void main(String[] args){
	String input = "5 2 +";
	String input_2 = "3 7 + 2 *";
	String input_3 = "5 1 2 + 4 * + 3 -";
	try {
		System.out.println(evaluate(input));
		System.out.println(evaluate(input_2));
		System.out.println(evaluate(input_3));
	} catch (Exception e) {
		System.out.println(e.getMessage());
		e.printStackTrace();
	}
  }
	
  public static Double evaluate(String input) throws Exception{
	Stack<Double> stack = new Stack<Double>();
	String[] tokens = input.split(" ");
	
	for (String currentToken: tokens){
	  if (currentToken.equals("+") || currentToken.equals("-") || currentToken.equals("*") || currentToken.equals("/") ){
		try {
		  double operand_2 = stack.pop();
		  double operand_1 = stack.pop();
		  if (currentToken.equals("+")){
			stack.push(operand_1 + operand_2);
		  } else if (currentToken.equals("-")){
			stack.push(operand_1 - operand_2);
		  } else if (currentToken.equals("*")){
			stack.push(operand_1 * operand_2);
		  } else {
			stack.push(operand_1 / operand_2);
		  }
		} catch (Exception e){
		  System.out.println("There is not enough operands for each operator.");
		  e.printStackTrace();
		}
	  } else {
		stack.push(Double.valueOf(currentToken));
	  }
	}
	
	if (stack.size() == 1){
	  return stack.pop();
	} else {
	  throw new Exception("Invalid Expression");
	}
  }
}
{% endhighlight %}

{% highlight python %}
Output: 
7.0
20.0
14.0
{% endhighlight %}

In conclusion, an evaluator for RPN is faily easy to implement. In my opinion, it is easier to implement than a normal mathematical expression (Infix notation) as there is no need for the parser to account for any parentheses. One downside to writing RPN is that it can be confusing at the start as most of us grew up using Infix notation for our entire lives. I guess it needs alot of getting used to and practice to write complex RPN expressions. 

Below are some links that i found helpful,   
<http://www.cs.csi.cuny.edu/~zelikovi/csc326/data/assignment5.htm>
<https://en.wikipedia.org/wiki/Reverse_Polish_notation>

