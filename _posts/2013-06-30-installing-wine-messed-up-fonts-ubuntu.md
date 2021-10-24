---
id: 584
title: 'WINE installation Messed up fonts [Ubuntu]'
date: 2013-06-30T14:13:48+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=584
permalink: /installing-wine-messed-up-fonts-ubuntu
categories:
  - Linux/unix
---
A known problem that _I myself experienced_, when I logged into Facebook, after rebooting to complete the installation of WINE. A tool that aims at _enabling windows programs to run on other platforms_.

So, what actually happens is that WINE installs a number of Microsoft Fonts, and well,  I guess, one of them happens to be on Facebook&#8217;s _alternative fonts,_ (the default one being Lucida Grande, which is **NOT included** in the package installed by WINE).

The problem can be fixed by simply uninstalling the package &#8216;msttcorefonts.&#8217; You can do this either via the &#8216;Ubuntu Software Center&#8217;, or the terminal.

To do this via the terminal, use:

**sudo apt-get remove msttcorefonts**

&nbsp;