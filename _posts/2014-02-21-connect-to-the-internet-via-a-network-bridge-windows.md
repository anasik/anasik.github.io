---
id: 1037
title: 'Connect to the Internet via a Network Bridge [Windows]'
date: 2014-02-21T17:04:59+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=1037
permalink: /connect-to-the-internet-via-a-network-bridge-windows
categories:
  - Web and dev
  - Windows
---
_You&#8217;ve got 2 computers at your place. A nice laptop, and a desktop that&#8217;s deprived of a wireless adapter. The laptop connects to the internet through the router, to which it wirelessly connects, however the PC has to do without internet since the router&#8217;s a bit too far away from the computer for a wired connection. _

In such a case, you might wanna consider (or might already have unknowingly) going for a network bridge. The idea is that you connect your computer to the laptop via an ethernet cable,  and the laptop to the router (WiFi). So in simple terms, you are connecting to the internet through the laptop which is connected to the internet.

But it&#8217;s not as simple as simply plugging in a cable. Some _configurations _have to be made, so here&#8217;s what you can do:

Open the &#8220;Network and Sharing Center&#8221; on your Laptop, and click on the &#8220;Change Adapter Settings&#8221; link. Select/Highlight both the Wireless and the Ethernet adapter, right-click, and click on &#8220;Bridge Connections.&#8221;  
Now sit back and relax while Windows sets up the bridge and once done, check if another icon appeared among the adapters, representing the &#8220;Bridge.&#8221; Also, you might wanna check if both the two adapters are part of the Bridge. It&#8217;s pretty simple and all you gotta do is see whether or not it says _&#8216;bridged&#8217; _ next to _enabled _on the adapters&#8217; icons. If either of them doesnt, right-click and select &#8220;_Add to Bridge.&#8221;_

Before you proceed, on your laptop, fire up CMD, and type in &#8220;**ipconfig /all**&#8221;, Note down the values it returns for _&#8220;IPv4 address&#8221;, &#8220;Default Gateway&#8221;, &#8220;Subnet Mask&#8221;_ and _&#8220;DNS Server.&#8221;_

Now the Client-side configuration. On the other computer, go to the Ethernet Adapter&#8217;s IPv4 properties; check &#8220;Use the Following IP address.&#8221; Fill in the &#8216;_Subnet Mask&#8217;, &#8216;Default Gateway&#8217;, and &#8216;DNS Server&#8217; _fields with the values you previously copied after running the ipconfig command on your laptop. However, in the IP Address field, add 1 to the last three digits, so that e.g. 168 becomes 169. As for the &#8216;_Alternate DNS Server&#8217; _field, add 1 to the last digit just like you did for the IP Address. That&#8217;s it; check &#8220;_Validate Settings upon exit&#8221;, _click &#8220;_ok&#8221;,&#8221;_ok&#8221;,&#8221;_close.&#8221; _ After a few seconds, the other computer would be connected to the internet.