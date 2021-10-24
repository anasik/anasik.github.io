---
id: 3657
title: Complimenting Complements
date: 2018-03-23T08:41:18+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=3657
permalink: /complimenting-complements
categories:
  - Essays
---
> This post assumes familiarity with &#8220;_Two&#8217;s Complement&#8221;, &#8220;One&#8217;s Complement&#8221; _ and an understanding of &#8220;Positional Numeral Systems&#8221;.

**Why is the Two&#8217;s Complement called the Two&#8217;s Complement?**

Ever wondered why the two&#8217;s complement and the one&#8217;s complement are named as such? We were told that to calculate the two&#8217;s complement of a number, you add 1 to its one&#8217;s complement. But why? When calculating the one&#8217;s complement, we simply subtract every digit from 1, so why don&#8217;t we subtract every digit from 2 in two&#8217;s complement? And maybe, you tried to reason with yourself about how there&#8217;s no &#8220;2&#8221; in the binary system and so that mehtod would not work anyway. There is however a much better way to understand and explain it. To understand this, you need to understand the difference between the _radix complement _and the _diminished radix complement. _

According to wiktionary:

  * The **radix complement **is the number which, when added to an n-digit number in radix-r, results in r^n. An alternative way of looking at it is that it is the smallest possible (n-1)-digit number in radix-r. The radix complement for radix-r is called r&#8217;s complement. We get it by adding 1 to the diminished radix complement.
  * The **diminished radix complement **is the number which, when added to an n-digit number in radix-r results in r^n -1. An alternative way of looking at is is that it is the largest possible n-digit number in radix-r. The diminished radix complement for radix-r is called (r-1)&#8217;s complement. We get it by subtracting every digit from (r-1)

<!--more-->

In radix-2, the radix complement is the two&#8217;s complement and the diminished radix complement is the one&#8217;s complement. Let&#8217;s try it for an arbitrary byte. The one&#8217;s complement of (11001011) is (00110100). If we add them together, we get (11111111) which is the largest 8-digit binary number. The two&#8217;s complement however is (00110101). If we add it to (11001011), we get 100000000 which is the smallest 9-digit binary number.

Let&#8217;s try the same with the popular and intuitive decimal system. Let&#8217;s take 589 as our arbitrary decimal number. The diminished radix complement of 589 aka the nine&#8217;s complement would be 410. If we add 589 and 410, we get 999 which is the largest possible 3-digit decimal number. The radix complement aka the ten&#8217;s complement would be 411. If we add together 589 and 411 we get 1000 which is the smallest possible 4-digit number.

Just to clear concepts, let&#8217;s try ternary(or trinary aka radix-3) as well. The radix complement would be the three&#8217;s complement whereas the diminished radix complement would be the two&#8217;s complement. Our number is 1021. It&#8217;s two&#8217;s complement will be 1201. Adding together 1021 and 1201 we get 2222 which is the largest possible 4-digit number in radix-3. It&#8217;s three&#8217;s complement will be 1202 which, when added to 1021 gives 10000 which is the smallest possible 5-digit number in radix-3.

You must have noticed how the method for calculating the two&#8217;s complement in radix-3 was different from the method for calculating the two&#8217;s complement in radix-2. Because in binary, the two&#8217;s complement is the radix complement whereas in ternary, it is the diminished radix complement. The two share the same name but are completely independent of one another.

**Two&#8217;s Complement vs Two&#8217;s Complement Notation**

People, especially CS undergraduates, confuse the two when they are actually two distinct concepts. A junior at my university once told me that she once got a question in an exam in which she was supposed to write the two&#8217;s complement of 27 and according to her, it was a trick question because 27 cannot possibly have a two&#8217;s complement as two&#8217;s complements exist only for negative numbers. I was like what?

Every binary number has a two&#8217;s complement. The two&#8217;s complement of 1011 is 0101 and the two&#8217;s complement of 0101 is 1011. It has nothing to do with negative numbers. Then what is the deal with negatives having two&#8217;s complements?

Well, computers have different ways of storing and identifying negative numbers and one of them is called two&#8217;s complement notation. In two&#8217;s complement notation, for n digits, all numbers between 0 and 10^(n-1) represent positive numbers and all numbers from 10^(n-1) to 10^n -1 represent negative numbers. So all binary numbers that start with a 1 represent negative values of their twos complement. in 4 digits, 0111 represents 7 while 1001, its two&#8217;s complement, represents -7.

In binary, 27 is 011011, whose two&#8217;s complement is 100101. In two&#8217;s complement notation, 27 is still 11011, however -27 will be 100101.