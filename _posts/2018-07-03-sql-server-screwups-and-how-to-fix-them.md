---
id: 3854
title: SQL Server screwups and how to fix them.
date: 2018-07-03T21:28:44+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=3854
permalink: /sql-server-screwups-and-how-to-fix-them
categories:
  - Web and dev
  - Windows
---
So recently I installed Visual Studio 2017 on a laptop and I installed it with only the ASP.NET and WinForms features. I was working on a webapp and initializing the database when I realized that VS was completely unable to connect to the database. I opened the &#8220;SQL Server Object Explorer&#8221; and tried to manually connect to the MSSQLLocalDB instance and I got this error.

> A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections. (provider: SQL Network Interfaces, error: 50 &#8211; Local Database Runtime error occurred. Error occurred during LocalDB instance startup: SQL Server process failed to start.) (Microsoft SQL Server)

![](https://scontent.fkhi2-1.fna.fbcdn.net/v/t1.15752-9/36485888_1304298333039828_9188284589292912640_n.jpg?_nc_cat=0&oh=25e438d56c465cdeaf2636a326aae8d3&oe=5BAB4949) 

I tried to start the service from the CMD and I kept on getting similar errors. Eventually, I concluded that the installation was corrupted and therefore tried reinstalling LocalDB. Guess what? It got me past this error but introduced me to another. It was something like this.

> CREATE FILE encountered operating system error 5(Access is denied.) while attempting to open or create the physical file &#8216;c:\Users\AnasDatabaseName-asdf5sdfasd5fs5dfs5f.mdf&#8217;.

This time, the CREATE Database command was failing because it was trying to create and MDF file in my &#8220;Users&#8221; folder. I checked the default location for the databases and it was in a sub-sub-sub-sub-folder inside AppData. A bit of Googling told me that this was actually a bug in the program itself and had been fixed in a recent update. So I just downloaded the latest Cumulative Update from the Microsoft site and it did the trick.