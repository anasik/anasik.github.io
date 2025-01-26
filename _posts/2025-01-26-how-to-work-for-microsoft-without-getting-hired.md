---
title: How to work for Microsoft without getting hired
author: Anas Ismail Khan
layout: post
permalink: /how-to-work-for-microsoft-without-getting-hired
categories:
  - Uncategorized
---
I've been a fan of open-source longer than I have been an adult. The moment I first switched to Linux in 2012, I knew it would remain my daily driver until I could afford to buy a Mac. As I, both voluntarily and involuntarily, continued to ditch my usual programs in favor of free and open-source alternatives, the deeper I dove into the world of open-source and the stronger became my desire to contribute to it. <!--more-->

I didn't have to wait long because between the Septembers of 2014 and 2015, I led the development of Emu-OS — An ubuntu-based distro for retro-gaming. This was a short-lived but two-pronged high because by then, building an operating system had also already been my dream for a couple of years. Sadly, just when Emu-OS started gaining popularity, my team abandoned me and I was too young to know how to continue on my own. Since then, I’ve come a long way, both as a developer and a professional, but Emu-OS remained my sole open-source contribution until just a few months ago. 

Last May, I started architecting a project in C# which revolved around OData — a standard maintained primarily by Microsoft for building RESTful APIs with powerful querying capabilities. In fact, *I particularly chose to build it in C# because of that.* It was the right decision because while there's fairly official OData libraries for both Java and Javascript, they are ridiculously lacking when compared to `Microsoft.AspNetCore.OData`.

It was practically our first day taking that library for a test-drive when a fellow coder showed me a bug. Remember when I said earlier that OData has *powerful querying capabilities*? 

```
randomapi.com/Products?$select=ProductID,ProductName,Price,Category&$filter=Price gt 20 and Category eq 'Electronics'&$orderby=Price desc&$top=10&$skip=5&$expand=Supplier&$count=true
```

The above query would fetch the ID, Name, Price and Category of the top 10 Electronic Products whose price exceeds \$20, sorted from highest to lowest price. Notice the part that says `$expand=Supplier`? That's a join with the Suppliers table. Powerful stuff right? 

Now let's talk about the bug. My teammate noticed that if you try to `$select` a field that was housing a `Collection<String>`, it would crash. I recreated the bug and immediately noticed that the problem was specific to SQL databases. If you had a mock database in-memory, or if you were to convert the `IQueryable` to a `List` it would work fine. I also noticed that while on SQL Server it was happening only with string collections, on SQLite it was happening with numeric types too. This made perfect sense since Primitive Collections were not even supported before EF Core 8, however it was also clear that this wasn't defined behavior. 

I decided to clone the repo for `Microsoft.AspNetCore.OData`, use a package reference in my project and debug through its lines. As early as the first run, I was able to isolate the problematic part of the code because a quick glance at the variables in the debugger showed me exactly the kind of LINQ that was part of the error message. Long story short, here's what was happening:
While collections were handled separately there was absolutely no discrimination in terms of base types. The same code was powering collections of navigations, complex and primitives. For each element of the collection it was performing a nested select like this:

`collection.Select(field => field.Select(d=> d))`

 While this makes perfect sense for navigations and complex types, for primitives it meant absolutely nothing and theoretically couldn't even be executed. ~~What's weird to me is that on SQL Server, integers could handle this when strings couldn't despite the fact that on a spectrum of scalar to collection, strings lean towards the collection side.~~ I grabbed a list of all the primitive types as defined in the release notes for EF Core 8 and added a condition where if the underlying type in the collection was any of those, it should proceed without the nested select. Worked like a charm. 

[I opened my PR on July 16th 2024.](https://github.com/OData/AspNetCoreOData/pull/1282) When it failed to catch attention, [I created an issue for the bug two days later](https://github.com/OData/AspNetCoreOData/issues/1286). That did the trick. Throughout August, I worked with the maintainers to make sure the code was up-to-standards, and that all tests were passing and everything was well-documented and more importantly, correctly documented. While I got approved a couple of times in September, it wasn't until the latter half of October that the nitpicking stopped. In November, I reached out to them and requested that they merge it because my project depended on it and they heeded my request. 10 days later, [the first release with my commits](https://github.com/OData/AspNetCoreOData/releases/tag/8.2.6) came out. 

Those of you who have made it this far, I want to thank you for taking the time to read this. As I finished typing the last paragraph, I realized that it reads like an excited child wrote it. But that's exactly how I feel. I've been a developer for the past 13 years, worked with people from various countries, written code in every major language, and made a lot of money, but very few experiences have felt as rewarding as this.