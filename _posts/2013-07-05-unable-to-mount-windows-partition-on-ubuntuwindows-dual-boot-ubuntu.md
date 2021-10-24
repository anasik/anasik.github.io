---
id: 590
title: 'Unable to mount Windows partition on Ubuntu&#038;Windows Dual-boot [Ubuntu]'
date: 2013-07-05T08:02:15+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=590
permalink: /unable-to-mount-windows-partition-on-ubuntuwindows-dual-boot-ubuntu
categories:
  - Linux/unix
---
Another known issue, no biggie. What happens is that when the person tries to mount a windows partition on Ubuntu, they get this huge error saying that &#8220;_the metadata kept in Windows cache refused to mount&#8230;&#8221; &#8220;the partition is in an unsafe state&#8221;  &#8220;not permitted&#8221; e.t.c._

Well, dont panic, the solution is present in the last few lines of the _&#8216;error message,&#8217; _only, not everyone realizes that, and often, it just doesnt work, _(that goes for Windows 8 users.)_

The reason behind it being the fact that your &#8216;Windows&#8217; might be in an hibernating state, a _proper shut-down_ ought to do the trick, but windows 8 users might continue to get this error even after a shut-down. This is due to the fact that Windows 8 comes with a _fast startup _ feature that really does speed up, by _not shutting down _completely. So you might be clicking on the shutdown button, but what really happens is that the _Windows _goes into a hibernating state.

So, here&#8217;s what you gotta do. _Login to Windows , _ and either disable the fast startup and shut-down properly, or you can do a normal restart, seeing as the _Fast Startup _doesnt apply to restarts.

To disable the fast startup feature, press start, to goto the start screen, and search(settings) for _&#8220;Power Options.&#8221; _Click on _&#8220;Change settings that are currently unavailable&#8221; (administrator access required) _and under _Shutdown settings, &#8216;un-check&#8217; _the _&#8216;Turn on fast startup&#8217; _button.

Thats it. Save changes and Shut-down.. or restart.