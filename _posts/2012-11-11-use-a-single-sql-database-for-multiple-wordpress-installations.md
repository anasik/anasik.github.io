---
id: 100
title: Use a single SQL Database for multiple WordPress installations
date: 2012-11-11T16:03:03+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=100
permalink: /use-a-single-sql-database-for-multiple-wordpress-installations
categories:
  - Web and dev
tags:
  - multipe wordpress installations
  - multpiple wp installations
  - single SQL database
  - Use a single SQL Database for multiple Wordpress installations
  - Use a single SQL Database for multiple Wp installations
---
Most people experience problems when trying to use a single SQL database for multiple WP installations. Its not as difficult as it seems to be. Start with uploading files, and carry on normally until you reach the install.php part.

At the bottom would be an input box labelled Table Prefix. By default, the value would be WP (which is most likely used for your previous installation.) So just replace it with something else, and continue. That&#8217;s it.