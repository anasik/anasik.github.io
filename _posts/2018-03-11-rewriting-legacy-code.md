---
id: 3808
title: Rewriting Legacy Code
date: 2018-03-11T02:37:16+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=3808
permalink: /rewriting-legacy-code
categories:
  - Essays
  - Web and dev
---
I once wrote an answer on Quora about the improbability of a programming language to completely die out once it has gained popularity. The idea, not so original, was that there are two factors preventing a language from dying:

  1. A significant amount of code is written in it and a significant amount of people or _other __code _depend strongly on it. An example is Linux. Now Linux has always been C and always will be. As long as Linux exists, it would be impossible for C to die out.
  2. Everyone knows about it. The problem with a programming language being popular enough for it&#8217;s existence to be common knowledge is that there will always be people who&#8217;ll be fond of it and would want it to never die out. The best example would be Richard Eng, the smalltalk evangelist who has grown more popular than the language itself and likes to call himself Mr. Smalltalk.

This post is about the first reason. C, PHP and JavaScript are three of the most popular and most hated languages that are getting closer and closer to being about as old as time itself. For my own reasons, I both like and dislike the 3 and also rely a lot on them. Let&#8217;s assume everyone agrees to killing all 3 of them. Now the problem is that C is:

  1. The language Linux is written in.
  2. The language most programmers start with. (Often this is because universities prefer to teach it in the earlier semesters.)
  3. More or less the only mature language that has the least amount of abstraction that one could ask for except for C++, which is a mess and an offspring of C itself and therefore not worth talking about in this post.

<!--more-->

Similarly, JavaScript and PHP are easily the languages that together make up the largest percentage of all the code on the web. JavaScript, being _the _language for client-side programming, is obviously inevitable in all but the most static of web-pages whereas PHP powers WordPress which powers more than 50% of all the blogs on the web and it&#8217;s one of the oldest, easiest, quickest-to-deploy, and the most practical, if not the best, language for server-side scripting.

If we kill them, the following things might happen:

  1. Blogposts about why you shouldn&#8217;t learn C or JS or PHP in 2018.
  2. People stop learning them.
  3. Less C/JS/PHP developers available per unit area.
  4. Organizations refuse to switch away from these languages due to the overhead cost.
  5. Supply and Demand 101: The 90s kids get better jobs as C/JS/PHP developers.
  6. Blogposts about why it might still be worth learning C/JS/PHP in 2018.
  7. C and JS and PHP live on.

But why? Let&#8217;s take a look at the events again. The reaction was fairly desirable until the 4th point. The reason behind wanting to kill the language was that people disliked it but here we&#8217;ve got people who&#8217;re hoping for it to live forever, and for what? Just so that they don&#8217;t have to spend any resources on switching, or should I say _upgrading,_ to a newer and supposedly better language. If we can tackle this sub-problem, we might actually be able to kill the languages after all.

How do we do that? By setting up an international body of standardized coding and to grant them the responsibility of rewriting all to-be-legacy code before killing a language.

Imagine a Linux Kernel and the whole GNU toolkit redone in Rust or Node.JS based Facebook. I like to call this _The Great Rewrite, _a historical moment in the timeline of computer-science progress when all developers puts aside their ego and pride and preferences and agree to do what&#8217;s best for everyone in order to ensure a better future of coding.