---
id: 808
title: 'Running StartX in Ubuntu screws up &#8220;login&#8221; [FIXED]'
date: 2013-11-29T12:05:24+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=808
permalink: /running-startx-in-ubuntu-screws-up-login-fixed
categories:
  - Linux/unix
---
Two days back, I out of curiosity, fired up a terminal and typed in startx, with the hope that the UI would be reduced to naught but the _original form _of the X. At first it displayed an error, saying that i&#8217;m not permitted to. But then I tried again, and this time with a **&#8216;sudo.&#8217; **The moment I _entered _my password, the screen went all dark, but only for a second, after which I was left with naught but a blank desktop, free from menus. Sensing failure, I opened another terminal and wrote **&#8216;unity.&#8217; **Something similar happened, but this time, the title bar of the terminal kinda got stuck to the top of the screen. I tried the same command again, and this time, I was brought back to where i started. The terminal in which i had typed the startx command was still there, however i failed to notice that the user logged into it wasn&#8217;t my primary user account but **root.** I didn&#8217;t really notice anything unusual, except that the _settings _dropdown, present in the top-right, in the gnome bar, wasn&#8217;t showing up the options it ought to, so I restarted the computer by simply pressing the power button.

After the reboot, whenever I&#8217;d attempt a login, I&#8217;d be greeted by a dark screen, and after like a fraction of second, I&#8217;d be back on the Login Page. But the guest account seemed to work fine, so I logged into it and created another Administrator account, and on that one, I tried recreating the problem and was successful. _Okay, _now we might get somewhere&#8230;

Now what I did, that fixed the problem, was this:  
I logged into the account, i.e the first one where i screwed everything up, via the tty1 terminal (Ctrl + Alt + F1), and ran the following commands:

<pre><strong></strong>sudo rm /home/anas/.Xauthority*
sudo apt-get install --reinstall xorg --fix-missing</pre>

The first one deletes the file, while the second re-installs Xorg. After this, I was able to login to the account again.