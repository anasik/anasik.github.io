---
id: 401
title: 'No hard drives been found &#8211; Linux XP/Red Hat'
date: 2013-04-16T12:48:22+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=401
permalink: /no-hard-drives-been-found-linux-xpred-hat
categories:
  - Linux/unix
---
An error i faced while trying to install Linux XP 2006 on a VirtualBox VM. The error is known to have been faced while installing Red Hat too, and the reason behind it is that neither of the two OS support SATA hard-drives, i.e.  at least the older versions didn&#8217;t, so I just unmounted the HDD from the SATA controller, and mounted the same HDD on the IDE controller. To do so in VBox,

  1. <span style="line-height: 15px;">Power off the machine</span>
  2. Goto the machine&#8217;s settings>Storage
  3. Right-Click on the HDD(under SATA in the storage tree), and click _Remove attachment_
  4. Add a hard-disk to the IDE controller and in the box that pops up, click &#8220;_choose existing disk&#8221; _
  5. Now all you gotta do is browse to the HDD you just unmounted and select it.

That&#8217;s it. Start the VM, it ought to work fine now.