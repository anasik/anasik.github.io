---
id: 587
title: 'MACBuntu messed up the Global Menu [Ubuntu]'
date: 2013-06-30T14:24:38+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=587
permalink: /macbuntu-messed-up-the-global-menu-ubuntu
categories:
  - Linux/unix
---
Ubuntu&#8217;s Unity interface greatly resembles that of MacOSX, and that includes _T__he Global menu_, AKA _The Mac Menu, _which shows the _Application menu(File, View, Edit, e.t.c.) _ in the _top bar. _  
__

Macbuntu, an _Ubuntu to MacOSX Transformation pack, _which i was so tempted to try, didnt work too well for me, and thus I uninstalled it. Yet, it left the scars; in this case, by permanently messing up the _global menu. _

This is how i fixed it. (terminal):

**sudo apt-get install appmenu-gtk appmenu-gtk3 appmenu-qt indicator-appmenu**