---
id: 1103
title: 'Animating Vegeta &#8211; The Power of CSS3 and Sprites'
date: 2014-05-24T09:54:10+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=1103
permalink: /animating-vegeta-the-power-of-css3-and-sprites
categories:
  - Essays
  - Games
  - Web and dev
---
[Check this out before you read any further.](http://www.anasismail.com/others/vegeta)

Some time back, I googled the term _&#8220;sprite&#8221;, [I tend to forget what it is (not anymore though)], _and in return was presented with a fine number of them, however, my eyes were quick to notice that the very first one of the results featured a few actions of Vegeta, one of the main characters from DragonBall Z. I thought it&#8217;d be cool if I tried creating a program, a web-page to be precise where one could animate Vegeta, by merely clicking on buttons.

So well, on May 21, around sunset, i got to work. I googled and checked out a number of sprites until I finally decided on [this one.](http://www.nes-snes-sprites.com/ImagesSheet/SuperButoden2Sheet3.gif)

Though I had initially decided to do it purely using JS, but then it hit me, that CSS3 supports animations so why not use that?  
Anyways, as soon as I began, I realized that this sprite is not very well suited to the job and thus I had no choice but to slice it up into several smaller ones, each representing a different _move. _I started with the &#8216;_stance&#8217;. _Made a four step sprite which initially was 176&#215;83. And tried animating it. Still the problem that remained was that I couldn&#8217;t get it to work like I wanted it to. What was happening was that the Image would just Slide from left to right. After like an hour or two I finally managed to get it right. Turned out that the number of &#8220;Steps&#8221; I was wasnt well suited to the initial and final positions  I had defined in the keyframes. In the end, I managed to get it right. I used 4 steps and set the final keyframes to _&#8216;-widthOfImage.&#8217; _That was it.  Now all I had to do was to create sprites for the other _moves._  
Next I tried the _&#8220;duck-and-punch,&#8221;  _which too worked like a charm with the existing code. But as I progressed, I realized that the size wasnt well suited to all actions. Some required a broader frame, while some _(initially) _required more than 4 steps, and that was when I somehow messed up the keyframes and wasted another hour fixing them. In the end, I decided to 4 steps where possible, (_I actually ommited some of the images), _and to use a 300&#215;87 canvas, where each frame was 75&#215;87.

Tried them all one by one by replacing the url in the code. All worked like a charm. Now I moved on to the &#8216;_kamehamehaa'(s). _The problem with these was that both the sprites were of 6 steps, so I had put them off for later. However the number of steps didnt make much difference except for the fact that I had to update the number of steps in the CSS , and resize the box everytime.

Now to the UI. What actually happens is that when you click on any of the buttons, it calls out a JS function which replaces the URL of the image with a different one, and thus a new sprite replaces the existing and thus is seen to perform a different _action. _Also, the function restores the original _&#8216;stance&#8217;_ sprite after the animation has completed one cycle.  
As for the 6-step Kamehamehaa(s), I wrote a different function and keyframes. Now this function though worked in a similar manner, it would update the animation attribute _(keyframes and steps)_ along with the image, and once completed one cycle, it&#8217;d restore the original keyframes and image. That was all.

As for the Energy bar, I declared a variable, initially equal to 50. Every Kamehameha decreases it by 10, while the rest increase it by 5. The Bar is updated every time the variable is updated, it&#8217;s width being directly proportional to the value of the var.