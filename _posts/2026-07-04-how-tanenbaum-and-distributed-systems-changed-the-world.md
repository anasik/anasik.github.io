---
title: How Tanenbaum (AST) and Distributed Systems Changed The World
author: Anas Ismail Khan
layout: post
published: false
permalink: /how-tanenbaum-and-distributed-systems-changed-the-world/    
categories:
  - Essays
tags:
- distributed-systems
- operating-systems
- computer-history
- linux
- python
- ethernet
- android
- minix
- amoeba
- sprite
- plan-9
- utf-8
- f2fs
- tcl
- tkinter
- xerox-parc
---
At the time of the writing of this post, hardly anyone who has any business reading this blog has any need for an introduction to Linux or Python or Ethernet or Android. I also admit that if you, the reader, are an avid enthusiast with either a computer-science degree or experience, you may be tempted to stop reading because you think you already know where this post is going. I urge you to keep reading because I'm certain this post has at least one fact to teach you. And even if it happens that you already know all of the facts presented in this post, it's possible that the beauty of the full picture would still not be lost on you.

**Andrew Stuart Tanenbaum**, more famously known by his handle, **AST**, is a Dutch-American ex-professor at VU Amsterdam. AST is primarily known for his work on microkernels that was immortalized in the form of MINIX, a UNIX-like operating system that he built mainly as a means for teaching by example. His book, *Operating Systems: Design and Implementation*, is considered legendary and a de facto standard for a complete and comprehensive understanding of operating systems principles. What makes this book unique is that it contains an abridged version of the actual MINIX code and almost a line-by-line commentary. While this was neither the first nor last book of this kind, it's certainly the most impactful.

In 1991, **Linus Torvalds**, a Finnish-American student at the University of Helsinki, bought this book and received a complementary copy of MINIX. While the book *lifted him to new heights*, as he quotes in his autobiography, he found the OS to be incredibly frustrating and restrictive. So, later that year, he went on to release Linux, a monolithic kernel designed to be paired with GNU, and the rest is history. But you probably already knew that. What you might not know is that this was neither AST's only contribution to modern mainstream software nor his only operating system. 

In 1978, **Sape J. Mullender**, author of *Distributed Systems*, a PhD student at VU at the time, supervised by AST, began what became **Amoeba**, a distributed OS *that appears to the user as a single, centralized time-sharing system*. In 1986, it became a joint-venture between VU and CWI (Centrum Wiskunde & Informatica), the Dutch national research institute for mathematics and computer science. In 1987, **Guido Van Rossum**, a research assistant at CWI, joined the Amoeba project. He felt that building system utilities in C was taking too long while the Bourne shell was too limited in its capabilities to be a viable replacement. He needed something in-between both.  Guido had previously worked on ABC, a failed programming language meant to dethrone BASIC. He decided to develop a new language that retains the strengths of ABC but improves on its shortcomings and thus **Python** was born. 

But Python was hardly the only lasting byproduct of a distributed systems project. In fact, it wasn't even the only scripting language to be credited as a byproduct of a distributed system. *What's more, Amoeba wasn't even the only distributed system credited for contributing to the modern Python ecosystem!*

In 1984, **John Kenneth Ousterhout**, a researcher at UC Berkeley, started working on Sprite. A UNIX-like distributed OS that had the same idea as Amoeba, but with a heavily-cached network filesystem and *process migration*, which allows programs to be moved between machines at any time. Ousterhout and his students had also built some design tools for integrated circuits. Since this was a pre-GUI era, commands were mainly invoked using a special language as opposed to button clicks and each tool had to have its own specially designed language. Having the genius idea of building one language to rule them all, Ousterhout started working on an *embeddable command language* in 1988. The idea was that the interpreter would provide basic programming features like variables and loops and you could include it as a package to other programs and add your own features. He called this language **Tcl (Tool Command Language)**. Recognizing the upcoming prevalence of GUIs, Ousterhout also built a GUI toolkit in Tcl around the same time. He named it **Tk**. 

On 31st May 1994, **Steen Lumholt**, a Danish programmer, posted the following to the Python mailing group:

> My Python interface to Tk/Tcl is kind of finished. The tk & tcl  
> library interface is finished. I'm still working on a set of Tk  
> wrapper classes.  
> 
> So now it's possible to create and manipulate Tk/Tcl programs from  
> Python. Python methods can also be registered as Tk/Tcl commands; so  
> that you don't need to write Tcl procedures!  
> 
> If you use Python in interactive mode, you can (as in wish) manipulate  
> the running GUI interactively. ...

The aforementioned Python interface is what we know today as **tkinter**, Python's primary GUI toolkit. And so, through tk through tkinter through Python, Tcl lived on and in exactly the role it was designed for: *embedded inside another tool*. What does it have to do with the Sprite project though? *Absolutely nothing, it appears!*

The Tcl language is often incorrectly credited to the Sprite project because both of them were Ousterhout's creations that appeared around the same time, and because Tcl was distributed through *sprite.berkeley.edu* via FTP. Furthermore, the *Tcl/Tk Engineering Manual*, which was released in September 1994 heavily referenced Sprite and even said, *This document is based heavily on The Sprite Engineering Manual.* However, just because Tcl wasn't a true byproduct of the Sprite project, doesn't mean that Sprite doesn't affect our lives today. 

One of the ideas that Sprite experimented with was *log-structured filesystems,* another brainchild of Ousterhout (and **Fred Douglis**), that treated the filesystem as a single contiguous append-only logfile, in the name of performance. In 2012, **Jaegeuk Kim**, an engineer at Samsung, introduced F2FS on the Linux mailing list as a flash-friendly filesystem for NAND-based storage such as SSDs, eMMC, and SD cards. It reused the core log-structured idea: write changes sequentially, then clean stale data later. Google’s Pixel 6-generation device tree mounts `/data` as **F2FS**, tying the idea directly to modern Android phones. By **2014**, F2FS was already showing up on Android hardware, starting with Google’s **Nexus 9** and became Google's recommended filesystem for the `/data` partition.

As it happens, *Sprite was not the only distributed system credited with producing a technology whose origin story is widely recounted incorrectly*.

While Berkeley was cooking Sprite, Bell Labs wasn't far behind and so in the late 1980s, **Plan 9** was conceived by **Rob Pike**, **Dave Presotto**, **Ken Thompson**, and **Howard Trickey**. Its first public design paper was presented at UKUUG in July 1990, introducing Plan 9 as a *distributed computing environment assembled from separate machines acting as CPU servers, file servers, and terminals.* This was supposed to offer greater efficiency than a network of general-purpose machines. It also deliberately avoided process-migration, deeming it unnecessary in the presence of efficient resource allocation. Unlike Sprite, it also did not try to present a single unified image of the whole network. Instead, Plan 9 extended UNIX's everything-is-a-file principle to devices, processes, network connections, and even the windowing system, then used mounts and binds to let each process assemble its own view of those resources.

In September 1992, merely days before the release of Plan 9's *first edition*, they were asked by the IBM representatives in the X/Open group, (one half of what became The Open Group as we know today,) to review a potential standard for *FSS/UTF.* Existing Unicode encodings were awkward for byte-stream systems: they wasted space for ASCII text, introduced byte-order problems, and could place null bytes inside C strings. FSS/UTF was meant to encode Unicode without breaking UNIX files, filenames, pipes, and tools. Seizing the opportunity, they instead offered to produce a superior standard. Their offer was accepted on the condition that they do it fast. According to Pike, this call came at dinner on a Wednesday. Thompson started coding right away and by early next day, they were already converting text files on Plan 9 to the new standard. By Friday, Plan 9 became the first operating system with complete support for **UTF-8**, the character encoding we cannot imagine life without today. 

UTF-8 was first documented for the IETF in RFC 2044 in October 1996, then revised by RFC 2279 in January 1998. Both traced it to the X/Open Joint Internationalization Group, named Gary Miller, Greger Leijonhufvud, and John Entenmann as original authors, and treated Thompson and Pike as later contributors. In an April 2003 email titled _UTF-8 history_, Pike addressed the prevalent misconception. Later that year, RFC 3629 corrected the record by crediting UTF-8’s design to Thompson and Pike, with Miller, Leijonhufvud, and Entenmann no longer mentioned.

All of these distributed systems projects belong to the same historical shift: computing had moved from isolated machines to networked machines. Amoeba, Sprite, and Plan 9 were attempts to redesign the operating system for the networking age. But before any of that became normal, someone had to make local networking practical. 

In 1970, Xerox created an R&D division called Palo Alto Research Center.  The lab's computer-science group was led by **Robert Taylor**, who had previously led the ARPANET project and **Alan Kay**, the creator of Smalltalk. Together with **Butler Lampson** and **Chuck Thacker**, who'd both worked on the Berkeley Timesharing System, they built the **Alto**, an experimental personal workstation with a graphical display, mouse, keyboard, local storage, and networking support. The Alto was arguably the first true modern personal computer. Lampson went on to describe the intended experience as distributed personal computing. Personal because every user had their own workstation and distributed because it relied on networking to share information and resources. 

Naturally, such a vision mandated the need for networking capabilities beyond what was already available. Lampson's 1972 Alto memo had already imagined networked Altos using an Aloha-like packet network over coaxial cable. In 1973, **Robert Metcalfe**, **David Boggs** and **Ed Taft** brought those ideas to life. This came to be known as **Ethernet**, the only wired networking standard that you and I have ever heard of. 