---
id: 914
title: 'Creating User Accounts via terminal [Ubuntu]'
date: 2013-12-21T16:55:03+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=914
permalink: /creating-user-accounts-via-terminal-ubuntu
categories:
  - Linux/unix
---
First off, simply fire up a terminal, and then for root access, type:

<pre>sudo su</pre>

It&#8217;d prompt you for the password, type it in.

Once you gain root access, type:

<pre>adduser <em>username</em><em>
</em></pre>

It&#8217;d ask you to _enter new UNIX password. _type one, and retype when it asks you to

That&#8217;s it. Now it would prompt you for a few more details regarding the new user, but they arent important, just press enter, and soon, it&#8217;d  prompt you to confirm if the information is correct. Type y and enter. That&#8217;s it. Your new user account is up and running. To delete it via terminal, type the following command when root:

<pre>deluser <em>username</em></pre>