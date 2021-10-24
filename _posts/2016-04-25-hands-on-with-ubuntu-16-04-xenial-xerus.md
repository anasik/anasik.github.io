---
id: 1267
title: 'Hands on with Ubuntu 16.04 &#8220;Xenial Xerus&#8221;'
date: 2016-04-25T20:39:58+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=1267
permalink: /hands-on-with-ubuntu-16-04-xenial-xerus
categories:
  - Linux/unix
  - Software, Devices, Reviews
---
Being one of those idiots who started downloading the ISO way before the link was even officially added to the download page, I do have a couple of reasons to regret doing so. I was on a slightly messed up 14.04 that appeared to have deteriorated over time, and I had been considering a reinstall, but had been putting it off because I had decided to wait until after the release of Xenial.

So, fast forwarding to when I was done installing it. As per habit, the moment it was installed I fired up a terminal and at the same time opened Firefox.. The first thing I noticed was that the terminal had a green font on the &#8220;_user@conputer:~$,_&#8221; and then I ran an apt-get update, which obviously was stupid as it had just been released, and a while before or after it I also noticed that the terminal seemed to insist that I use &#8220;_apt&#8221; _in commands in place of _&#8220;apt-get.&#8221; _I don&#8217;t honestly know what inspired this change, but just another minor.

Two changes that we had been hearing about since way before the release were: <!--more-->the ability to move the launcher, and the microphone volume slider in the volume dropdown in the gnome panel. I didn&#8217;t bother to try the former since I happen to like my launcher on the left, but I was hurt when I realized that the Mic volume slider wasn&#8217;t there in my installation. (W

_hat&#8217;s funnier is that it was actually there in the live boot._) Yes, the session buttons are there in the dash. (_yay! -_-_)

Then, after I had installed chrome, which was all I needed at that time, I _tried _to feel at home with the new Ubuntu.. for some reason, minor changes and bugs only prevented me from doing so. Take for example the fact that the screen flickers every now and then.. this happens to be an issue that I faced once before on 12.10. According to [this](http://askubuntu.com/questions/760319/screen-flickering-on-ubuntu-16-04-lts), it could actually be a kernel issue, so I have chosen to wait, _(although I won&#8217;t mind trying an upstream kernel, but I am kinda short on time.) _Other than that, every time I wake my laptop, the WiFi icon in the gnome panel get&#8217;s replaced by the two &#8220;opposing arrows&#8221; i.e. the ethernet sign.. _Okay.. a messed up icon shouldn&#8217;t be an issue right? but no wait.. every time that happens, it also stops listing WiFi networks, and it also stops showing the network I am connected to (although I stay connected.) _To get past this, I fire up a terminal and run:  
`sudo service network-manager restart`

But, despite all this, I was more or less inclined to make the best for it and wait for the first few batches of updates, when I remembered the one thing that I had actually noticed the moment it booted up after the install, but had chosen to look into it later, the fact that attempting to change the brightness using the Fn keys resulted in uncontrollable flickering. I plan to do a post later about what I tried and how I got past the issue.

Oh, and I read in a few articles about how it now mounts system partitions as removable drives, and I never really understood what that was supposed to mean until I tried it. _Yes, it&#8217;s stupid, buggy, and it sucks. _Every time I mount any of hard-disk partitions from nautilus, it creates an icon in the launcher, and while you are _inside_ the mounted partition, that icon stays in the launcher and stays highlighted as if it&#8217;s a separate application with it&#8217;s running instances independent from nautilus. And when you unmount it, you see a stupid notification that says &#8220;_You can now unplug XGB Volume.&#8221; _It can all be pretty messy at times. Sometimes the icon doesn&#8217;t disappear from the launcher even when it has no reason to stay there, and rarely you&#8217;d accidentally open a new nautilus window without meaning to.

In short, I do sometimes find myself wishing that I had, at the very least, waited at least a week. None of the issues are a major bug in the OS anyway.. some of them could be unique to my computer. All I _can _do now, is _wait. _