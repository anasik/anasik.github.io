---
id: 388
title: 'Get rid of the &#8220;This Copy of Windows is not genuine&#8221; error in Windows 7 Ultimate'
date: 2013-04-05T17:02:18+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=388
permalink: /get-rid-of-the-this-copy-of-windows-is-not-genuine-error-in-windows-7-ultimate
categories:
  - Windows
---
Often when you&#8217;ve got updates on, there comes a time when your desktop background disappears, and you keep on getting errors and popups saying that your copy of windows is not genuine, and stuff like &#8220;you might be a victim of software counterfeiting.&#8221; The reason behind is that you are using a pirated version of Windows which you didn&#8217;t **_obtain the legal way._**_ _Now the point is that these errors and popups can be annoying, and people would always welcome a cure, and well here&#8217;s one. I&#8217;m not encouraging anyone to go for pirated software, but i&#8217;m still posting the solution cause not everyone can afford to buy their own licensed copy of windows.;-)

So getting rid of the error is quick, simple and clean though a bit annoying as you need to restart twice in a row.

  1. Goto Windows update and click on the link on the bottom left corner, saying &#8220;installed updates.&#8221;
  2. Look for the update KB971033, and uninstall it (right-click>uninstall,) and reboot.
  3. Run Command Prompt as Administrator. To do so, just press start, type cmd, and on the first link, right-click>Run As Administrator.
  4. In the CMD window, type the following command: _slmgr.vbs -rearm_, and press enter.  At first, nothing would happpen, but this is normal, but after a few _seconds, _a box would pop up, saying that you need to reboot for the changes to take effect. Well, do as it says.

> Note: This would only rid you of the WGA notifications and popups, but remember, this wont fool it into believing that you are using genuine windows, means that you&#8217;d still be unable to use the Security Essentials.