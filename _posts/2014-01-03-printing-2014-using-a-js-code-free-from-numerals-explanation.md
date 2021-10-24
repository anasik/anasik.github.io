---
id: 967
title: 'Printing 2014 using a JS code free from numerals [Explanation]'
date: 2014-01-03T07:35:39+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=967
permalink: /printing-2014-using-a-js-code-free-from-numerals-explanation
categories:
  - Web and dev
---
Came across [this thread](http://codegolf.stackexchange.com/questions/17005/produce-the-number-2014-without-any-numbers-in-your-source-code) on stack exchange. The challenge was to write a code, using whatever language you want, that would print the number 2014 to the console, without using a single numeral in the code. The person to post the code with the least number of characters wins.

After scrolling a whole lot over a number of Befunge codes and all, I came across a JS code, where the guy had said &#8220;_Using pure-Math only..&#8221;, _and he&#8217;d posted two codes, one used some type-conversion while the _older version _ was like in plain JS. This:

    m=Math;p=m.pow;t=true;++t+m.floor(p(m.exp(m.PI),t))*t*t++-p(++t,t--)/--t

Since the guy had written that the code was pure-math, I decided to give it a closer look.  
As I was going to try it out, I opened another tab, and in the console, I pasted his code. It did really print out 2014, but how? It&#8217;s been ages since I even printed my own name in JS, so figuring out the code took me quite a while.  
In the address bar i typed &#8220;data:text/html, <html contenteditable>&#8221; to make it editable, as i wanted to make notes while trying shit in the console.

So, the first thing I did was add a little whitespace to the code and make it slightly more presentable.The code is basically 4 _lines_, should i say? Notice the three semicolons? yes, they ensure that the code wont screw up if you give a line break right after the semicolon.  
The first bit is _&#8220;m=Math&#8221;_. This the guy did simply to save space and perhaps for his own ease. Math.function() is the syntax for performing and implementing in your code various mathematical functions, rather like the Math module in Python. Followed by _&#8220;p=m.pow&#8221;,_ and _&#8220;t=true.&#8221;_ _m.pow_ is his code&#8217;s version of _Math.pow(x,y)_, which sets the index of parameter x to y, so _Math.pow(2,3)_ would be equal to 2³. This too was merely to save space. The _t = true_ could be confusing for some. That&#8217;s when JSs automatic type-conversion comes in handy, _(True = 1; 0 = False)_. So _&#8220;t-true&#8221;_ sets _&#8220;t=1.&#8221;_

Now the main expression:

    ++t+m.floor(p(m.exp(m.PI),t))*t*t++-p(++t,t--)/--t

The ++t sets the variable _t= t+1._ I&#8217;ts the same as _t += 1_ in Python. You might have noticed that the code also contains &#8220;_t++_&#8220;, so what&#8217;s the difference between the two?  
_t++ _increases &#8216;t&#8217; by 1, but does not implement therein. Means if the code is _t=3; t++*3; _The first bit set&#8217;s &#8216;t&#8217; equal to 3, and in the second, t is multiplied by 3 while at the same time increased by one. ++t is slightly different. It increases and implements right then. So _++t*3_ would result in 9 not 6.  
Then the m.floor function is used to round off a number to the last integer. So 5.9 would become 5. Math.exp(a) results in 10ª, while Math.PI is a constant, which of course is equal to 3.141592653589793.

That&#8217;s pretty much all the functions explained. After starting with the very _deepest _bracket, and working my way out to the end, I ended up with the following expression that&#8217;s equal to &#8216;2014&#8217;, and is like a simplified version of the original code. this:  
<span style="background-color: #f4f4f4; font-family: 'Courier 10 Pitch', Courier, monospace; font-size: 13px; font-style: normal; line-height: 1.5;">2+(1070*2-Math.pow(4,4)/2).</span>

To see how I got there, [here&#8217;s a copy of the page on which I was making all the notes.](http://ubuntuone.com/18l2qnhls8tThUjG7Gp2Di)