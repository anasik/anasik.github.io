---
title: Third time's the charm
author: Anas Ismail Khan
layout: post
permalink: /third-times-the-charm/    
categories:
  - Tech
  - Software
  - Personal
  - dotnet
  - csharp
  - aspnetcore
  - open source
  - Microsoft
  - Essays
  - Web
  - Dev
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
> Also check out:
> 1. [How to work for Microsoft without getting hired](https://anasismail.com/how-to-work-for-microsoft-without-getting-hired/)
> 2. [Oops, I did it again](https://anasismail.com/oops-i-did-it-again/)

It's no secret that now I'm a contributor to open-source and that the fruit of my Pull Requests is being distributed by none other than Microsoft themselves. Earlier this year, I posted twice about bug fixes I'd made to `Microsoft.AspNetCore.Odata`, and I'm pleased to inform you that as of today, I have achieved a hat trick.

I must admit though, that this bug was a lot subtler and less detrimental to the primary function of the library. In fact, viewing strictly by the OData standard, it cannot be called a bug at all. However, the library offers a feature or two that deviates from the standard. And it was one of those features that was broken.

The OData specification defines dollar-prefixed query options e.g. `$top`, `$skip`, whereas the `Microsoft.AspNetCore.Odata` library generously offers a configuration flag, enabled by default too, called `EnableNoDollarQueryOptions` that allows you to use non-dollar-prefixed query options like `?top=10&skip=10` instead of `$top=10&$skip=10`.<!--more-->

Of course, it's no coincidence that I used `top` and `skip` as examples just now. The neat thing about these two options is that they're designed to work together for what's aptly termed *client-driven pagination*. The `top` option tells the API how many records to fetch and the `skip` option sets the offset. So if you want to fetch the first 10 records, you do `?top=10&skip=0` or simply `?top=10` but then to get the next page of records, you'd do `?top=10&skip=10` and so on. In most cases, your top would stay pretty constant while your skip increment each time. 

However, outside of their collective function, they still have some interesting properties. For that, we must understand the concept of *server-driven pagination*. Simply put, the server can impose a limit on the maximum number of records that can be fetched at once. That's known as `PageSize` and it's also often the default number of records returned when there's no `top` in the query. 

In the event that the `top` value is greater than the `PageSize`, the API caps the number of records at `PageSize` and includes a `@odata.nextLink` in the response for the user to get the remaining records. So if `PageSize` is set to 100 and the user queries with `?top=1000`, they would essentially go through 9 `@odata.nextLink`s to get all 1000 records. Let's look at some examples now. Say, your make a request to:
```
api.anasismail.com/odata/Products?$top=1000
```
you'd get back a `@odata.nextLink` that looks like:
```
api.anasismail.com/odata/Products?$top=900&$skiptoken=Id-'100'
```
Because you've already gotten the first 100 records and need only another 900 and not another 1000. As you can guess, once you've traversed your way past the first 900 records, the last 100 records do not come with a `@odata.nextLink` in the response. 

Again, it's no coincidence that I put that `$skiptoken` in the request above. This is an option, no different from a regular `$skip` in its purpose, that most servers return instead of the regular `$skip`. This is mainly done for performance reasons as it allows you to take advantage of a default sort on the collection you're paginating. See how it says `Id-'100'`? That tells the server to fetch all records with `Id > 100`. In contrast, a regular `$skip=100` would quite literally skip the first 100 records in the database instead of taking advantage of the indexed `Id` field. For larger skip values, this becomes very slow.

Now let's see how `skip` works when not paired with a `top`. If you were to make a request to:
```
api.anasismail.com/odata/Products?$skip=1000
```
You'd get back a response with the following `@odata.nextLink`:
```
api.anasismail.com/odata/Products?$skiptoken=Id-'1100'
```
See how the server not only removed the `$skip` option from subsequent queries but also incremented the `$skiptoken` by the `$skip` value to correct for the skipped records? Beautiful. And now, we can finally talk about the bug this post is about.

I noticed that if you call the API with a `top` (not `$top`) value greater than `PageSize`, the `@odata.nextLink` in the response was reiterating the same `top` value instead of decrementing it by `PageSize`. So, a request to:
```
api.anasismail.com/odata/Products?top=1000
```
gave back a `@odata.nextLink` that looked like:
```
api.anasismail.com/odata/Products?top=1000&$skiptoken=Id-'100'
```
This would basically lead to infinite `@odata.nextLink`s. Similarly, I noticed that when I called the API with just a `skip` (not `$skip`), the `@odata.nextLink` in the response was not removing the `skip` from the `@odata.nextLink` in the response such that a request to:
```
api.anasismail.com/odata/Products?skip=1000
```
would give back a response with the following `@odata.nextLink`:
```
api.anasismail.com/odata/Products?skip=1000&$skiptoken=Id-'1100'
``` 
This would cause, what can only be described as, a *double-skip* since the resultant query would both fetch by `Id > 1100` and then *also skip the first 1000 records in the results*. 

Fixing this was remarkably easy. I found the file that generated the `@odata.nextLink` values. It had a huge `switch` with cases for `$top` and `$skip`. I just added `case "top":` and `case "skip":` under their respective cases and made a PR. I did wonder if I should check and see if `EnableNoDollarQueryOptions = true` before matching these cases but thought the odds of someone using these keywords for any other purpose were slim. 

However, the reviewers were of a different opinion and so I had no other choice but to do `case "top" when isNoDollarQueryOptionsEnabled:` and `case "skip" when isNoDollarQueryOptionsEnabled:` instead. And to make that work, I had to pass `isNoDollarQueryOptionsEnabled` all the way down the hierarchy from a lot of places where this function was being called. 

Then they said that a test was failing. I could have sworn that I'd been careful enough in my testing and that no test could possibly fail. That being said, I also knew I hadn't run any of the tests before requesting their review. So I investigated and turns out, as expected, it was the test that was bad and not my code. I fixed the test, pushed, requested review and BAM! Hat trick!