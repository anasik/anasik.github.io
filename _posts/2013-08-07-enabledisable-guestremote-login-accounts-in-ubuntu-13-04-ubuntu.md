---
id: 707
title: 'Enable/Disable Guest/Remote Login accounts in Ubuntu 13.04 [Ubuntu]'
date: 2013-08-07T13:04:05+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=707
permalink: /enabledisable-guestremote-login-accounts-in-ubuntu-13-04-ubuntu
categories:
  - Linux/unix
---
The guest and the _remote login _accounts are enabled by default, but there are those who want them disabled, and this could be due to privacy concern (though i noticed that u cant do much in the guest account), and as for the remote one, well, maybe they just dont want to see it there.

Well, to disable them, all you gotta do it fire up a terminal (Ctrl + Alt +T), and paste in the folllowing command:

### To Disable both:

**sudo /usr/lib/lightdm/lightdm-set-defaults -l false -R false**

### to disable **only **the guest one:

**sudo /usr/lib/lightdm/lightdm-set-defaults -l false**

### to disable the **remote login**:

**sudo /usr/lib/lightdm/lightdm-set-defaults -R false**

That was it; ought to do the trick.

> To **Enable, **use the same commands, however, make a li&#8217;l alteration; i.e. replace all the &#8216;_false&#8217;s_ with _true. _