---
id: 3805
title: 'Comparing strings with &#8220;==&#8221; operator vs &#8220;.equals&#8221; method [Java]'
date: 2018-02-22T14:09:19+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=3805
permalink: /comparing-strings-with-operator-vs-equals-method-java
categories:
  - Web and dev
---
During lunch, I was reflecting on that day when my programming teacher asked me to come forward and teach &#8220;bitwise&#8221; operations to the whole class, and I remembered how, in my attempt to explain to them the basic difference between a regular &#8220;OR&#8221; and a bitwise &#8220;OR&#8221;, I had talked about value comparison being different from a bitwise comparison. Then I began to wonder. A bitwise operation on machine level is fairly simple to understand, but what about value comparison? What is it that happens at machine level when you check if two integer values are equal?

As I began my search for the answer, I pinged Vladislav Zorov, my mentor and friend, and asked him. He said that compilers mostly just subtract the memory addresses of the two objects being compared and returns true if the difference is zero i.e. if they are literally the same object. I couldn&#8217;t agree with this so I argued against it. I said that it is possible for two variables to point to identical objects without pointing to the same object and so I set out to prove it.

I wrote a very simple program:

> import java.util.Scanner;  
> public class HelloWorld {  
> public static void main(String []args){  
> // Created two strings using the same literal and an array with the second index set using that literal.  
> String x = &#8220;abcd&#8221;;  
> String y = &#8220;abcd&#8221;;  
> String[] z = {&#8220;asafaf&#8221;,&#8221;abcd&#8221;};
> 
> // Printing addresses of all 3.  
> System.out.println(Integer.toHexString(x.hashCode()));  
> System.out.println(Integer.toHexString(y.hashCode()));  
> System.out.println(Integer.toHexString(z[1].hashCode()));
> 
> // Checking to see if the equality symbol works on these  
> if(x == z[1] && x==y && y==z[1])  
> System.out.println(&#8220;== works on the 3&#8221;);
> 
> // Comparing the 3 using the equals method which will, obviously, work.  
> if(x.equals(z[1]) && x.equals(y) && y.equals(z[1]))  
> System.out.println(&#8220;equals method works on the 3&#8221;);
> 
> // Creating Scanner object to read from stdin.  
> Scanner input = new Scanner(System.in);
> 
> // Taking two strings as input. Will test with  
> // 1. Two different strings  
> // 2. Identical strings  
> // 3. &#8220;abcd&#8221; for both  
> String j = input.next();  
> String k = input.next();
> 
> //printing their addresses  
> System.out.println(Integer.toHexString(k.hashCode()));  
> System.out.println(Integer.toHexString(j.hashCode()));
> 
> // Testing both comparisons  
> if(j==k)  
> System.out.println(&#8220;== working on input strings&#8221;);  
> if(k.equals(j))  
> System.out.println(&#8220;equals working on input strings&#8221;);
> 
> // Initializing a string identical to x,y,z[1] but with the new keyword  
> String a = new String(&#8220;abcd&#8221;);  
> // Printing its address  
> System.out.println(Integer.toHexString(a.hashCode()));  
> if(a==x || a==y || a== z[1])  
> System.out.println(&#8220;== working with new keyword&#8221;);  
> if(a.equals(x))  
> System.out.println(&#8220;equals method working with new keyword&#8221;);  
> }  
> }

If you run it, you&#8217;ll see that its output shows that all strings that have the same value point to the same address regardless of how they&#8217;re initialized. But the equality symbol only works when two strings have been created with the same literal. The fact that we&#8217;ve now proven that identical strings in Java do have the same memory address does perhaps imply that comparison is done simply by comparing addresses but then what about the equality symbol? Well, I&#8217;ll update this post when I find out.