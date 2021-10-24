---
id: 312
title: '&#8220;no root specified&#8221; error during ubuntu installation'
date: 2013-03-17T07:22:30+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=312
permalink: /no-root-specified-error-during-ubuntu-installation
categories:
  - Linux/unix
---
While installing ubuntu 12.10, when i got to the partition-ing part, i kept on receiving an error saying something like &#8220;no root file system is defined.&#8221; After countless attempts, i figured out  a solution to this.

Firstly, make sure that the partition where you are installing ubuntu is an ext4 partition, and  dont leave _mount point _empty. To do so, just double click the partition, and in the box that pops up, select the ext4 journalling file system, and in mount point, just add a single &#8220;/&#8221;

That&#8217;s it! you should be able to proceed now.