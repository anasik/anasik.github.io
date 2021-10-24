---
id: 398
title: 'Recover Partitions after Uninstalling Linux &#8211; &#8220;Not enough space on the disk to complete this operation&#8221; Disk management'
date: 2013-04-16T12:28:28+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=398
permalink: /recover-partitions-after-uninstalling-linux-not-enough-space-on-the-disk-to-complete-this-operation-disk-management
categories:
  - Linux/unix
  - Windows
---
Often users face this problem, after uninstalling Linux, and re-partitioning the _un-allocated space. _What actually happens is that they keep on getting stupid errors saying &#8220;Not enough space on the disk to complete this operation,&#8221; whenever they try to create new partitions using the Disk Management tool in Windows.

The reason behind being that the Windows Disk Manager is not&#8230; Ah _capable _of creating partitions in the middle of the disk, and thus, it displays the error. Users who created the Linux partitions at the end of the HDD, wont face any such difficulties.

Anyways, the solution is to go for some _advanced partitioning tool _e.g. GParted which is for Linux. There might be a _Windows_ _compatible _version, but I never was successful with my searches, so you might wanna go for the Ease Us Partition Master, which comes with a lot of advanced features, and would certainly get you across this error.