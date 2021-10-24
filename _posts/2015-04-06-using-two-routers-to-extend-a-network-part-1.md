---
id: 1198
title: 'Using two routers to extend a network &#8211; Part 1'
date: 2015-04-06T09:08:41+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=1198
permalink: /using-two-routers-to-extend-a-network-part-1
categories:
  - Software, Devices, Reviews
  - Uncategorized
  - Web and dev
---
Umm, yeah, so let&#8217;s get to it. What was the first interpretation? oh that&#8217;s right, Router B to act as a wireless access point for A.

So, A has an internet connection and B has to be connected to it via a cable and configured in such a manner that the connected devices automatically connect to either of the two devices with the best signal as you move about, and as B is acting as an access points, all data B receives and sends would of course need to be sent to and received from A. (_Pardon me if something I&#8217;ve written doesn&#8217;t seem correct, I&#8217;m merely a noob and explaining in terms your grandma could understand._)

This was actually pretty simple, so I&#8217;d just list the steps leaving out the screenshots.

  1. Get an ethernet cable and insert one end of it into any LAN port on A, and the other end into the first LAN port of B. (_actually I&#8217;m not sure if it **has to be** the first port or not._)
  2. Login to the web interface of B and set the SSID, i.e the name of the network, and the security settings of B to be the same as those of A. e.g. if A is called &#8220;narlges&#8221; and it&#8217;s using WPA, with passphrase &#8220;flutterwacken&#8221;, then you need to apply the same settings on B.
  3. Making sure that both A and B are in the same subnet, change the LAN IP adress of B to something other than that of A. So if the IP of A is 192.168.0.1, then you can set B to 192.168.0.X. Basically X can be any number between 0 and 255 except 1 as it is being used by A.
  4. Disable DHCP on B as it won&#8217;t be assigning IP addresses and all.
  5. Other wireless and radio settings like channel and all need to be the same too
  6. Reboot both routers?

And basically that&#8217;s it.