---
id: 780
title: Window Managers vs Windowing Systems(Display Server) vs Desktop Environments
date: 2013-10-28T11:56:13+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=780
permalink: /window-managers-vs-windowing-systems
categories:
  - Linux/unix
---
If u happen to be a Linux user, you might, at the very least be familiar with the Desktop environments: GNOME, KDE, LXDE or XFCE. _Also, _you might have come across the names _OpenBox, X Window System _and _Window Maker, _and perhaps an implementation of more than one of these _together, _e.g. &#8216;Gnome/OpenBOX.&#8217;

Now the Question is, If they are all UIs, what exactly is the difference? And how can they all be working together at the same time? Because despite how similar they seem, each serves a totally different purpose.

To explain this, I&#8217;d take the example of layers. Imagine a stack of, let&#8217;s say, papers. The _Stack _represents a GUI Linux OS, and every individual sheet of paper represents a different component. Now assuming that the bottom section is dedicated to the GUI Components, the layer at the very bottom of this section would represent the X-Window System. It like forms the _base _of the GUI, and displays the information in a graphical way. Above the X, is a Window Manager, i.e. _assuming, _OpenBox. This Window Manager, as it&#8217;s name states, helps you _manage _them open windows. Menus, Taskbars, managing the look and feel, e.t.c. could be examples of what it might offer. Now a Window System, the X, may offer some of the functionality of a Window Manager, but still, they are two different things. In Simple words, a Window manager only enhances the Windows System.

Now the Desktop Environments are a bundle of Applications and tools, with their own UI, and built on some Window Manager, that provide the user with the essentials like a File Manager, Text editor, Browser, e.t.c.

Now to sum it all up, take the example of UbuntuGnome. The Ubuntu Gnome uses the X Windows System as the display server, but on top of it is the Openbox, and the Desktop environment is Gnome, which comes with its own set of Applications like the Nautilus File Manager, Gedit(text-editor), Rhythmbox Music Player, e.t.c.

&nbsp;