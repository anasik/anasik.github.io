---
id: 1274
title: Fixing the brightness issue on Ubuntu 16.04
date: 2016-05-09T11:03:37+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=1274
permalink: /fixing-the-brightness-issue-on-ubuntu-16-04
categories:
  - Essays
  - Linux/unix
  - Software, Devices, Reviews
---
  * The issue: Random flickering when changing the brightness using the function key, while the change wasn&#8217;t steady. The slider in system settings allowed me to change the brightness normally.
  * The machine: Dell Inspiron N5110

The first solution I tried was creating the /usr/share/X11/xorg.conf.d/20-intel.conf file with the following lines:  
`Section "Device"<br />
Identifier "card0"<br />
Driver "intel"<br />
Option "Backlight" intel_backlight"<br />
BusID "PCI:0:2:0"<br />
EndSection`

This didn&#8217;t change anything. So I tried following _&#8220;dushnabe&#8217;s&#8221;_ suggestion on [this thread](http://askubuntu.com/questions/476664/cannot-change-backlight-brightness-ubuntu-14-04). Which too didn&#8217;t make any difference really. The problem, as I saw it was that I appeared to be using both intel\_backlight and acpi\_video0. Both use different ranges of values to change the brightness. Hence the flickering. It became clear that I had to force the usage of just one, and that&#8217;s exactly what the fix in that answer was supposed to do. Except that for some reason it wasn&#8217;t working.

After googling further on this, I landed on [this page](https://wiki.archlinux.org/index.php/backlight) and I saw the list of kernel parameters that had to do with the backlight. I rebooted a couple of times, each time trying a different parameter, and finally,  
`acpi_backlight=native` is what did the trick. I noticed that it doesn&#8217;t allow me to change brightness on login screen, but after login, there was no flickering, and when I ran `ls /sys/class/backlight/`, I saw that it no longer returned acpi_video0. The only issue I have right now is that there is no fixed minimum. Sometimes, it decreases to a reasonable minimum, while at other times, it results in a blackout, and I have to manually adjust it using the slider in system settings or using xbrightness..

To replicate this process, all you need to do:

  * Fire  up a terminal
  * sudo nano /etc/default/grub
  * At the very end of the string GRUB\_CMDLINE\_LINUX_DEFAULT, (which in my case was &#8220;quiet splash,&#8221;) add `acpi_backlight=native.`  
    The final string, in my case, looks like &#8220;`quiet splash acpi_backlight=native`&#8220;
  * Close and save the file, and run `sudo update-grub` and then reboot.

> In the event that this doesn&#8217;t work, it&#8217;d be worth your time to try out the rest of the kernel parameters. You don&#8217;t have to modify the grub file every time. Instead you can choose to modify kernel parameters before boot. This you can do by pressing &#8220;c&#8221; on the grub screen and typing the desired parameter, in the correct place, right after _&#8220;splash.&#8221;_