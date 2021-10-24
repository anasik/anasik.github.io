---
id: 1030
title: Creating a Counter-Strike Server using Port Forwarding and the dedicated-server application
date: 2014-02-21T16:06:38+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=1030
permalink: /creating-a-counter-strike-server-using-port-forwarding-and-the-dedicated-server-application
categories:
  - Software, Devices, Reviews
  - Windows
---
First off, to create a server, you need additional files. The main executable of them being the **&#8220;hlds.exe&#8221;,** and if this one&#8217;s present, we must assume that the other prerequisites are present too, (including **swds.dll,** which is like a patch that allows non-steam clients.) If not present, just search for them, and download them.

Creating a server on a machine is no big deal. All you have to do is run the HLDS, and fill in the slots with whatever you wish, _(who am I to limit the max no. of players on your server?) _and I assure you that the slot for _Server Name _can carry anything. However, in the _**&#8220;UDP Port&#8221;** _slot, type **&#8220;27015&#8221;** since it&#8217;s the preferred (actually most-widely-used) port for CS. The Server&#8217;s up the moment you click <button style="background-color: #4c5844; color: white; border: gray inset 1px; box-shadow: 1px 1px 1px black;">Start Server.</button> But the thing is that the server&#8217;s local. It&#8217;s accessible only on your PC and on other computers on the same network, but not globally accessible over the internet. _So how do we make it global?_

That&#8217;s what we forward the ports for. To do so, login to your router, by typing the router&#8217;s IP address into the browser. TP-LINK users may consider this a complete step-by-step guide, though of course this applies to all, however different ones have slightly different UIs.

Anyways, once logged in, goto **_Forwarding_** and then to **_Virtual Servers,_**_ _and click on <button>Add New</button>. A form should greet you. Now in the Service port slot, type &#8220;27015&#8221;, (if there&#8217;s an &#8220;_internal port&#8221; _slot, leave it blank.) The rest can be left to the defaults, however in the IP Address Slot, type in your computer&#8217;s ipv4 address, [and it should be static](http://anasismail.com/how-to-assign-a-static-ip-to-on-windows-computer/ "How to Assign a Static IP to on Windows Computer") (unless you are looking forward to having to go through the whole damn procedure again and again.)  
After inputting the IP, save the changes and now you are good to go.

Now others can connect to your server by typing into their CS consoles:

<pre>connect xxx.xxx.xxx.xxx:27015</pre>

(Replace &#8216;xxx.xxx.xxx.xxx&#8217; with your router&#8217;s global IP address. To find out the global IP, the quickest and easiest way is to visit [whatsmyip.org](http://whatsmyip.org).)