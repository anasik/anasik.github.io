---
id: 2523
title: Kaspersky OS
date: 2016-11-15T14:07:51+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=2523
permalink: /kaspersky-os
categories:
  - Linux/unix
  - News
  - Software, Devices, Reviews
---
> _First_, it’s based on [microkernel architecture](https://en.wikipedia.org/wiki/Microkernel), which allows to assemble ‘from blocks’ different modifications of the operating system depending on a customer’s specific requirements.
> 
> _Second_, there’s its built-in security system, which controls the behavior of applications and the OS’s modules. In order to hack this platform a cyber-baddie would need to break the digital signature, which – any time before the introduction of [quantum computers](https://en.wikipedia.org/wiki/Quantum_computing) – would be exorbitantly expensive.
> 
> _Third_, everything has been built from scratch. Anticipating your questions: not even the slightest smell of Linux. All the popular operating systems aren’t designed with security in mind, so it’s simpler and safer to start from the ground up and do everything correctly. Which is just what we did.

Let&#8217;s talk about this. _Micro-kernel design?_ Interesting, but MINIX has had that for ages now. Linux vs MINIX = Monolithic vs Microkernel = Performance vs Security. Yes, going for one kernel design instead of the other does equal compromising one aspect for the other. In short, this decision to use the micro-kernel isn&#8217;t honestly innovative.

Built-in security system? Oh wow.. Sure, whatever. Give us more details and then we will consider it&#8217;s existence and efficiency.

Everything has been built from scratch? I admire your effort, but at the end of the day, it is going to have to be POSIX compatible. It&#8217;s hard to say whether or not it really was worth the effort. And I hate to break this to you, but it would have saved time, and made more sense, to _proofread _ the code instead of rewriting it.

In short: As of now, it offers nothing too interesting. Sure, I&#8217;d like to download an image and give it a go but that&#8217;d probably be it.