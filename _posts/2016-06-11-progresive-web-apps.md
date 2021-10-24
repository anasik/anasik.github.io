---
id: 1312
title: Progresive Web Apps
date: 2016-06-11T20:32:18+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=1312
permalink: /progresive-web-apps
categories:
  - Essays
  - Web and dev
---
You are using a computing device, be it a smartphone, a tablet, a desktop computer. It&#8217;s new, shiny, with little or no applications installed, _apart from the bloatware that the manufacturer could have generously shipped with it. _You fire up Facebook in a web browser, like a couple of pictures, post a status, have a small chat with a friend, and then after a while, you close the tab and lock your phone. After a while you do it again, and this time, you spend a whole hour scrolling through the news feed, and then once again you close the tab, and lock your device.

Now while it&#8217;s locked, and still connected, your device makes a decision. Assuming that you like Facebook, it adds a Facebook icon to your homescreen, or your app-drawer, for easy access to facebook.com. So the next time you unlock your iPhone, you simply tap on that icon, and it opens facebook.com in your default web browser. You love it.. It&#8217;s just a simple link, but it already feels great, and it could be better. Soon enough, after another day&#8217;s usage of the site, you notice that tapping the app icon no longer opens a browser window with facebook.com. Instead, you get a window solely running Facebook like it&#8217;s a standalone native application for your operating-system. <!--more-->On a smartphone, it would look just like the Facebook app does, and on a desktop, it would look like the smartphone app running inside a re-sizable and drag-gable window. Furthermore, it would now be capable of running in the background. Meaning you get push notifications for activity on Facebook. On desktop operating systems, there could be a Facebook icon in the system tray. Now on that computing device, Facebook is about as native and as much of an application as the stock music playing app.

And _that, _is what the idea of _&#8220;Progressive Web Apps&#8221; _is. Yes, the name is pretty straight forward. Facebook, an app on the web, progressed for you, from a tab in your default browser, through a link on your homescreen, to an app running natively on your device capable of running in the background and sending push notifications. The beauty of it? the user never had to fire up an app-store or download a package file in order to install it. The computer itself computed that the user liked using the web-app enough to want to have a standalone app.

The description is more about the experience, than about what those apps are and how they would work, and it is vague about certain details, that would lead to questions like, for example, what if the user didn&#8217;t want it to progress to a link in the first place? Well, obviously in a world dominated by PWAs, such concerns would obviously be seen to. A user could enable/disable the automated progression of web apps. Furthermore, upon visiting a webapp, a user could be prompted with a choice like &#8220;Use facebook.com often? Add it to your homescreen.&#8221; Such prompts too could be disabled as per the wish of the user.

So now that we have a decent idea of what user-experience with PWAs could be like, let&#8217;s move on to the next obvious topic. _Development. _The questions that come to one&#8217;s head when they try visualizing the experience, and my answers to them are as follows:

**Would all web-apps be PWAs?**

My honest answer? Yes. On iOS and Android, you can already add links to websites to your home page. But whether or not every dynamically coded website could transform into a standalone native application is what remains to be answered. This of course means that all of the work towards accomplishing this is to be done on the OS&#8217;s part. In short, Operating Systems should handle the progression. They should have a program that allows for websites to be transformed into PWAs. It&#8217;s pretty simple actually, all that has to happen is that the browser engine has to run in a window without any of the browser&#8217;s controls. The static parts of the webapp&#8217;s code could be downloaded so as to allow for offline usage where possible, and this would update in real-time when you&#8217;re connected.

Mimicking and replacing the functions of an appstore, Operating Systems could have, lists of websites that they see as PWAs, and these would be the ones that would, based on the user&#8217;s settings, either install automatically, or prompt the user with a choice for the install.

**How does it help the developers?**

Well, in short, developers won&#8217;t need to create standalone apps for every platform. They just create a sick looking, amazingly responsive, and addicting-ly functional web-app, while the different operating systems would have their own ways of handling it&#8217;s progressions.  
Of course that makes it sounds like the developer would have no control over what their standalone apps for different platforms are like. So, allow me to add that this would be totally upto the developer. If they do feel like going through the trouble of making a sick looking smartphone app, that&#8217;s a little more than a website running without browser controls, then they have my permission to do so. This obviously means that for some apps, there&#8217;d be one final stage in the progression. The downloading and installation of the standalone app provided by the developer, and users could even choose whether or not to go for it.

**Notifications?**

In the first part of the article, I talked about how, in the final stage of the progression, the app would be &#8220;native,&#8221; and capable of sending push notifications. I mean if facebook.com has progressed to the stage where it runs like a standalone app in it&#8217;s own window, and can run in the background, every time anything changes, you could receive a notification like &#8220;Activity on Facebook!&#8221; which is okay, but wouldn&#8217;t it be better if the notifications were more precise?  
Obviously that would require some work on the developer&#8217;s part. One way is for them to simply develop a decent standalone application themselves.  
Other than that, it&#8217;d be pretty simple for the engine to allow for webapps to change the default text for notifications, (for example: a basic messaging app could change it to &#8220;New Message!&#8221;) and well, where complexities are involved, demanded and desired, they could easily configure the app to use the notifications API to work as needed.

**But what&#8217;s the point?**

Well, in the grand scheme of the unification of operating systems, PWAs could be the final trigger. One of the reasons as to why new smartphone operating systems don&#8217;t take off is the unavailability of apps. Standalone apps are always developed for major platforms, and even then developers may choose to not bother about particulars. So for example, some people wouldn&#8217;t want an Ubuntu phone simply because it can&#8217;t run whatsapp.  
Dominance of PWAs can, however, change this. Reason? All apps would primarily be web-apps. They may or may not have especially developed standalone versions. So yes, their prevalence could lead to us getting decent webapps for WhatsApp or Instagram.  
Web-apps in general are the thing of the day. Brilliant small ideas pop up in our newsfeed every now and then, and some take off pretty quick and never have to land. Those overtime usually get smartphone apps, but in a possible future, the developers would never have to develop the smartphone or desktop apps. They would just, _implicitly _exist.