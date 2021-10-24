---
id: 1034
title: How to Assign a Static IP to on Windows Computer
date: 2014-02-21T16:33:50+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=1034
permalink: /how-to-assign-a-static-ip-to-on-windows-computer
categories:
  - Web and dev
  - Windows
---
A Static IP differs from a Dynamic one in the sense that the latter changes with every router reset, while the former always stays the same.

To change your IP to a dynamic one, follow these steps:

  1. Fire up CMD, and type in &#8220;ipconfig /all&#8221;
  2. Note down the values it returns for &#8220;IPv4 address&#8221;, &#8220;Default Gateway&#8221;, &#8220;Subnet Mask&#8221; and &#8220;DNS Server.&#8221;
  3. Open Network and Sharing Center (quickest and simplest way is to right-click the network icon in the system-tray and clicking on the link therein.)
  4. In the sidebar on the left, click on the &#8216;<em style="color: #000000;">change adapter settings&#8217; </em>link.
  5. Double-click on the icon of whatever adapter you are connecting to the internet through.
  6. A window would pop-up. Right in the centre of the <em style="color: #000000;">box, </em>would be a list of <em style="color: #000000;">items </em>that the connection makes use of. Look for &#8220;<em style="color: #000000;">Internet Protocol Version 4(IPv4)&#8221; </em>in the list; select it; click properties.
  7. Another box would pop-up, but this one&#8217;s the last in the row. Now this step is where you make the real changes, but before you change anything, check if the &#8220;<em style="color: #000000;">Use the following IP address&#8221; </em>radio button is already checked, and if it is, quit straight away since it&#8217;s already static. However if not, then do check it.
  8. At least three of the previously disabled input fields would now be enabled. Fill the fields with the details you copied from the results to the ipconfig command. <span style="font-family: Georgia, 'Bitstream Charter', serif; font-style: italic;">The last bit of the IP Address is variable and you can give it any value you wish, however, copying the existing would do the trick too.</span>

That&#8217;s it you are done.