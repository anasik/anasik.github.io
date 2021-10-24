---
id: 3886
title: I made a MIPS simulator
date: 2019-01-13T10:49:21+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=3886
permalink: /i-made-a-mips-simulator
categories:
  - Software, Devices, Reviews
  - Web and dev
---
> AKA MIPS &#8211; A Java Based MIPS simulator. [Browse the code on GitHub.](https://github.com/moiz-frost/AKAUI)

Long story short, my Data Structures teacher gave us this project where we hasd to use our knowledge of data structures to make something useful. She said we could do it in groups so naturally I found myself in a team of 3.

She told us about the project near the start of the semester and a week before it was due we were still trying to decide what we were gonna make. She said that we would have to present our project and pitch it and convince the audience that it&#8217;s a useful piece of shit. The problem with us was that any idea we came up with was either far too advanced and therefore not worth the time and effort or far too simple for our ego to allow us to go about presenting it as our grand project.

I thought maybe we could make like a virtual machine of the Altair or any other primitive computer and that led us to the idea of making a MIPS simulator. We opened the instruction set in a browser tab immediately and were relieved to find that it was sufficiently small and therefore we decided that this was what we wanted to do.

Within 24 hours I had written a buggy but functional parser that could read assembly files coupled with a machine object using a couple of integer arrays for simulating the RAM and registers. Downloading and running some assembly code for MIPS on my parser helped me fix some bugs and typos that had crept in.

Because the teacher had insisted that the project be a graphical program, one of our team members was tasked with creating a GUI for this and so he made a JavaFX project that ended up looking very similar to the MARS simulator for MIPS. No it wasn&#8217;t at all a coincidence because he had used MARS before.

After coupling my code with the GUI, I would say we ended up with a pretty decent program. It has a few limitations, e.g. lack of floating-point support. I learned from Steve Wozniak that it&#8217;s okay to leave that out when you&#8217;re writing your own language processor. On a serious note though, I didn&#8217;t initially plan on leaving it out but I learned after it was a bit too late that MIPS had a whole set of 64 bit registers for handling floating-points and I didn&#8217;t feel like making any changes to the otherwise working parser. Other than that, there&#8217;s no support for unsigned arithmetic. The whole program is Java based and Java doesn&#8217;t have unsigned types so neither does the simulator.

[The code for the program is available on github](https://github.com/moiz-frost/AKAUI). Feel free to check it out, give your feedback and even suggest improvements. It&#8217;s not on my teammates account because he created an pushed the code for the GUI on a separate repo and then merged the parser code directly into it.