---
id: 1211
title: 'Using two routers to extend a network &#8211; Part 2'
date: 2015-04-28T13:53:56+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=1211
permalink: /using-two-routers-to-extend-a-network-part-2
categories:
  - Software, Devices, Reviews
  - Uncategorized
  - Web and dev
---
> **The goal:** Create two separate networks, each with its own router. Both routers will have different security and SSID, while the WAN settings of A are configured to connect to the internet while B, being a subnetwork of the first, will connect to the internet through it.

Now the thing is that the LAN and WAN IP addresses can not be in the same subnet, so here&#8217;s what I did. I changed the subnet of A from 255.255.255.0 to 255.255.0.0 .. Also, I changed to IP Adress to 192.168.1.1. That&#8217;s all the config you need to do in Router A, assuming it is already configured to connect to the internet.

Now get an Ethernet cable and plug one end of it into any of the LAN ports (some reccomend the first) in A, and the other end into the WAN port of B. Login to it&#8217;s portal.. yeah it&#8217;s at 192.168.0.1. Though I don&#8217;t see why dynamic shouldn&#8217;t  work, but since it didn&#8217;t for me, let&#8217;s assume it won&#8217;t work for anyone else. Select Static IP in the startup wizard and you&#8217;d be greeted by a number of blank input-boxes.  Fill them in as follows:

<p style="text-align: center;">
  IP Address: 192.168.1.2
</p>

<p style="text-align: center;">
  Subnet Mask: 255.255.255.0
</p>

<p style="text-align: center;">
  Gateway: 192.168.1.1
</p>

<p style="text-align: center;">
  Primary DNS Server: 192.168.1.1
</p>

<p style="text-align: left;">
  That ought to do the trick. You might want to do a reboot, but that&#8217;s not always necessary.
</p>