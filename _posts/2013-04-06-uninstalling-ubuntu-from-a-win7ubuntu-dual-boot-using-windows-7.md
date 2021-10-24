---
id: 391
title: 'Uninstalling Ubuntu From a win7&#038;ubuntu Dual-boot Using Windows 7'
date: 2013-04-06T16:33:57+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=391
permalink: /uninstalling-ubuntu-from-a-win7ubuntu-dual-boot-using-windows-7
categories:
  - Linux/unix
  - Windows
---
Ubuntu, based on Linux  though is free and open-source, not everyone would be satisfied with it, as an average user would get pissed off in a week, because there arent many of the desired apps available for Ubuntu  and those that are, people get a hard time installing them. I haven&#8217;t got anything against Ubuntu, and I&#8217;m not asking anyone to stop using it, but those that regret installing it in the first place, here&#8217;s how you can uninstall it.

<!--more-->But before starting, make sure that you&#8217;ve:

  1. <span style="line-height: 15px;">Backed-up all your data</span>
  2. Have got the installation media of either of the 2 OS

To repair using the Windows Install Disk, here&#8217;s what you gotta do:

  1. Press start and in the search bar, type &#8220;disk,&#8221; and from the results, select &#8220;Create and Format Windows Partitions.&#8221; Using the Disk Manager, delete the Linux partitions. Identifying them wont be difficult as Windows cannot read Linux partitions, therefore, the ones, for which, no info is available ought to be the ones containing Linux. Now Linux is gone, and so is Grub (most of it)
  2. Now reboot with your Windows installation disk, and run command prompt which is present in the repair/recovery tools.
  3. In the CMD window, type _**bootrec /fixmbr** _and press enter, followed by _**bootrec /fixboot**_** **and yes _enter._
  4. That&#8217;s it. Just reboot, and you should be able to boot directly to Windows.

But if you havent got the Windows Install media, you can also repair using the Ubuntu installation media. In the 2nd step, boot from the Ubuntu Disk, and once loaded, click &#8220;Try Ubuntu.&#8221;

Open a terminal and type the following command **sudo apt-get install lilo** followed by** ****sudo lilo -M /dev/sda mbr.**

> Replace &#8220;sda&#8221; with your partition

&nbsp;

&nbsp;