---
id: 1086
title: 'W: Duplicate sources.list entry [FIXED] [Ubuntu]'
date: 2014-04-27T20:58:33+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=1086
permalink: /w-duplicate-sources-list-entry-fixed-ubuntu
categories:
  - Linux/unix
---
Okay, so ever since I upgraded to Ubuntu 14.04 through the liveCD, i hadn&#8217;t once been prompted to update stuff. This seemed unusual to me as I used to get updates every now and then, and then the fact that I hadnt upgraded from 13.04 until right after the LTS was launched and the support for Raring had already been cut off. Also, the fact that I had aborted the installation before the upgrading process had completed.

Still, after a fair amount of days had past, last night i booted it up and ran the **sudo apt-get update** command. It started fine but then it would get to a part where it would get stuck for several minutes and then would display several occurrences of the same error:

     W: Duplicate sources.list entry http://xxxxx.xxx.xxxx xxx/xxx xx(x/x/x/x.x)

Pardon the X&#8217;s. The former part of the error is what matters. So, the first thing everyone does is a google search, and the best thread I could find on the subject was [this one](http://askubuntu.com/questions/120621/how-to-fix-duplicate-sources-list-entry). I tried creating a new sources.list file after backing up the original just like it says there, and tried again. Still got the same error, so I searched again and this time I used slightly different keywords. Landed on a site that suggested checking the &#8220;_other software_&#8221; tab in the &#8216;_software and updates_&#8216; settings for duplicates.  I checked and found that the &#8220;Canonical Partners&#8221; entry was recursive. I removed one and tried the old command again. And still, it didn&#8217;t make any difference.

Then I tried the &#8220;Y PPA Manager.&#8221; It actually has a tool that is supposed to search for and remove duplicates. Even that didnt work, so I decided to manually look for duplicates in the file.

The file wasn&#8217;t a pretty sight, so the first thing I did was to eradicate all those &#8220;_comments._&#8221; which left me with a fine list of sources. (sudo gedit /etc/apt/sources.list) Next thing, I looked for duplicates. e.g. if there was a line that said:

<pre>deb http://xxx.xxx.xxx xxx xxx main restricted</pre>

then there shouldnt be one that says:

<pre>deb http://xxx.xxx.xxx xxx xxx restricted</pre>

since the former includes both &#8220;_main&#8221; _and &#8220;_restricted._&#8221;

Since I wasn&#8217;t too sure of anything, I merely &#8220;_commented_&#8221; the lines that looked suspicious. (All you gotta do is add a &#8220;#&#8221; at the start of the line.) Once done disabling all possible duplicates, I saved the file and re-ran the update command. This time it worked!

> However, later on i noticed that the &#8220;Canonical Partners&#8221; repo had disappeared from the list present in the &#8220;other software&#8221; tab. This I then manually added to the sources.list. Just paste the following lines:
> 
> deb http://archive.canonical.com/ubuntu trusty partner  
> deb-src http://archive.canonical.com/ubuntu trusty partner
> 
> &nbsp;