---
id: 77
title: Moving The sidebar from right to left in the 2011 wordpress theme
date: 2012-10-28T07:34:55+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=77
permalink: /moving-the-sidebar-form-right-to-left-in-the-2011-wordpress-theme
categories:
  - Web and dev
tags:
  - Moving The sidebar from right to left in the 2011 wordpress theme
---
Most people experience this problem when editing the default Twenty Eleven theme and usually, they try placing the sidebar before the main content in the php file, or adding some code to the functions.php file. But they dont need to.  
Now there are 2 ways to do this. The simplest is:  goto Dashboard>Themes>customize>layout and select &#8220;Content on Right&#8221;

But if you wanna do it through CSS then open style.css,  and replace this code:

#secondary {  float: right;  margin-right: 7.6%;  width: 18.8%; }

with this one:

#secondary {  float: left;  margin-left: 7.6%;  width: 18.8%; }

&nbsp;

&nbsp;