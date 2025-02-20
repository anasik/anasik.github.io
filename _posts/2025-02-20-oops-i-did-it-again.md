---
title: Oops, I did it again
author: Anas Ismail Khan
layout: post
permalink: /oops-i-did-it-again     
categories:
  - Uncategorized
tags: 
  - Open Source
  - Microsoft
  - OData
  - C#
  - CSharp
  - Bug Fixing
  - ASP.NET Core
  - Entity Framework Core
  - Software Development
  - Programming
  - Contribution
---
A month ago, I wrote about my first contribution to an open-source, Microsoft-maintained, project, from June 2024, that got merged by November 2024. What I didn't mention at that time was that *that* wasn't my only contribution to that project. Shortly after that pull request got merged, I opened a second one. Once again, I created an issue, to report a bug, and a pull request, to solve the bug, back to back. However, this time I made them in the correct order. 

The same project from the last article required me to enable ETags on the OData API that I was working on. An ETag is an HTTP header that's used for cache-invalidation and concurrency control. It's basically like a hash representing the state of the resource/data at said endpoint. The client may use it as a cache key and the server can use it for concurrency control when multiple requests attempt to update the resource at the same time. ETag values are based on a special field/column on the resource record that changes every time the record is updated. 

Lets talk for a moment about how ETags help with concurrency control. <!--more-->There's a concept called *Optimistic Concurrency Control*. It's a non-locking mechanism that gets its name from the optimism of assuming that conflicts are very unlikely to occur. I like to think that it gets its name from the assumption that the user would be responsible enough to include the `if-match` header in the update request. This header, which holds the last known ETag value known by the client, is compared with the current value on the resource to detect conflicts. 

Caching works in a similar manner. Client passes the ETag in a header called `if-none-match` in a GET request. If the value matches, the server sends back a `304 Not Modified` status. Otherwise it sends back the whole record. This reduces bandwidth usage and speeds up the response time. 

In C#, a popular convention, but by no means a rule, for these concurrency check fields is to use a `byte[]` field called `RowVersion` annotated with a `[Timestamp]` or `[ConcurrencyCheck]` attribute. The `Microsoft.AspNetCore.OData` package has a lot of abstraction built in to simplify the generation and validation of ETags. If your model has concurrency fields set up correctly, it automatically picks them up and handles conversions. It even goes one step further and adds a field called `@odata.etag` to the JSON response, rather than just putting it in the headers. *And even more importantly*, it gives you a these two prebuilt methods for computing and comparing ETags:

```
queryOptions.IfMatch.ApplyTo(IQueryable query)
```

and

```
queryOptions.IfNoneMatch.ApplyTo(IQueryable query)
```

The `ApplyTo` method, which exists on the `ETag` class, has a `foreach` loop that iterates over all the concurrency properties and compares them one by one with the values on the actual record. As you might have already guessed, for each field, it was performing a simple equality comparison. Well and good, until you introduce array fields like 90% of the users like to. Meaning, the `IfMatch` and `IfNoneMatch` headers were definitely not having the intended effect for what was undoubtedly the most popular use-case.

Initially, I thought it was just me and that I had configured something incorrectly. To make sure, I decided to take a look at the existing E2E tests for the ETag class. Surprisingly, not a single field in the model for the test was of the type `byte[]`. I proceeded to add one and sure enough, the test failed immediately. I went back to the `ETag.ApplyTo` method and added a check inside the loop for array types. I added some code to handle the array condition and guess what? The tests passed.

I opened an [issue](https://github.com/OData/AspNetCoreOData/issues/1332) and a [pull request](https://github.com/OData/AspNetCoreOData/pull/1334) immediately. I added detailed instructions for reproducing the bug using the existing E2E ETag Test code. This happened at the very end of October. Since the team was busy creating releases for the already merged bugs, it took them as late as January to actually respond. They asked for more tests and I explained how I brilliantly modified an existing one. I thought they'd have a problem with me modifying an existing test since I myself didn't feel all that great about it but all I had really done was add a new field to the test and make sure that all the existing test cases were ignoring that field except for the one test case that actually needed it. 

Luckily, a few days from that, they approved my changes and then another few days after that, they merged it. They are yet to make a release that includes this fix; ~~because including this fix, there's hardly been any changes to that branch~~ however, I'm hoping it's just around the corner because we are forced to use a fork in our project, instead of the NuGet package, solely because of this bug.