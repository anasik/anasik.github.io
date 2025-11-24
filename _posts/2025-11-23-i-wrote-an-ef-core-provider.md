---
title: I wrote an EF Core Provider
author: Anas Ismail Khan
layout: post
categories:
  - dotnet
  - csharp
  - Microsoft
  - aspnetcore
  - open source
---
> EF Core Database Provider for Azure Data Explorer (Kusto). [GitHub](https://github.com/anasik/EFCore.Kusto)   [![NuGet Version](https://img.shields.io/nuget/v/EFCore.Kusto.svg)](https://www.nuget.org/packages/EFCore.Kusto/) 

The project I work on has a lot of data in a lot of entities with a lot of fields exposed through an OData API. Naturally, that compels us to know exactly what kind of query shapes our users may be interested in and to create optimal indexes for them beforehand. However, there's literally hundreds of fields and creating an index for every single one of them is simply not an option. Timeouts are great for protecting the database from maliciously demanding queries but they don't offer much in the realm of user-experience. We *need* to allow clients to send arbitrary queries and we *need* them to be fast regardless of whether we're expecting said query or not.

It was clear to me that we needed a solution and not an optimization and so I considered exploring Big Data solutions and columnar databases. Since our infrastructure is primarily Azure-based, my boss suggested that we explore Azure Data Explorer (ADX) aka Kusto. In a couple of days, ingestion was complete and a little testing showed promising performance so we decided to explore our options for integration. 

Sadly, Kusto not only doesn't have an EF Core provider package, the T-SQL support, although well-marketed, was poorly performing. It was very clear to us that this path would require a little investment. After discovering that `EFCore.Snowflake`, another library/database pair we considered using, is developed and maintained by one person alone, I felt motivated to take a crack at writing a database provider. I mean, how hard could it be in today's AI powered era? 

My boss decided to make things interesting and offered me a huge bonus in exchange for me accomplishing this in under three weeks. So on 18/11/2025, I started by carefully exploring the code for `EFCore.Snowflake`. I knew that my implementation would be far simpler since I didn't need to support design time services and OLTP flows. But at the same time, I also expected there to be complications surrounding the connection flow since Kusto doesn't support the ADO.NET architecture as the Kusto.Data library internally uses the REST API.

About 5 hours later, I could tell that it was functioning but I couldn't prove it because Kusto kept failing to parse the SQL parameters that my provider was failing to map. It was late so I went to bed with the goal of approaching this with a clear head.

The next day, I had to go down a much deeper rabbithole than I'd expected just to find a way to extract the parameter values early enough in the pipeline, cache them somewhere, and then map them once the KQL generation was completed. Even now, I'm not proud of how I handled it but it seemed to have worked. I was too excited to care anyway since I desperately wanted to see my odata api, (which is what I'd been using for testing,) return the correctly serialized output. 

Once parameters were fixed, I got my wish but now I had to make `$expand` work which is basically a fancy OData term for `JOIN`. This is the part where I'm most proud of myself because I ended up needing to do this with little or no AI intervention as LLM hallucination peaked around this time and I wasn't willing to put up with it.

Just in case you were wondering, yes of course I was vibe-coding this but not by aimlessly prompting cursor while I sip my coffee. No, I had ChatGPT open in a window and I was giving it very detailed instructions and asking very specific questions because this part of the .NET api is barely documented. In fact, the documentation for this literally has a disclaimer at the top stating *These posts have not been updated since EF Core 1.1*. I also learned soon enough that ChatGPT's knowledge of this was almost equally lacking and I was kind of on my own. 

Took me most of the next day to get `$expand` work, after which the KQL generation was near perfect but failing because of the `ROW_NUMBER` function that EF Core loves to use in subqueries and I had failed to detect and remove. Now there were a bunch of ways to go about fixing this, some more sophisticated than others, but KISS (Keep it simple, stupid) for the win! I decided to do exactly what I would do if I was the parser: explicitly look for it and remove it. 

And, just like that, my odata api was returning data from my Kusto cluster! There were, however, exactly three more challenges before I could call my boss and ask him to pay up.

1. In KQL there's no `$skip` or `.Skip()` equivalent. However, there is a `ROW_NUMBER()` equivalent which is simply `row_number(int startingIndex)`. I handled skips by simply projecting a `skip_index = row_number(1)` and then filtering by `| where skip_index > `. ~~And yes, I had a very good reason for not using this function to handle the `ROW_NUMBER()` issue around subqueries that I mentioned earlier.~~ To make this work, I also had to make sure that `order by` comes before `project` because `row_number()` is a window function and window functions can only be called on a serialized set, and `order by` always outputs a serialized set.

2. Next came `$filter`. I started basic and to my surprise, equality checks were crashing but inequality checks were working fine. This was because KQL uses `==` for equality unlike SQL's `=` so I had to override `GetOperator`. Naturally, other operators also had similar issues. `AND` and `OR` simply needed to be lowercase while `FieldName IS NULL` and `FieldName IS NOT NULL` simply had to be `isnull(FieldName)` and `isnotnull(FieldName)`. I had to override `VisitUnary` to make these two work btw.

3. Lastly, `$count` needed to work. Now the code was already emitting `| project  = COUNT(*)` which wasn't correct KQL, but it was perfectly incorrect for me to detect and replace. As a final-step, just before execution, I added a check that looks for this expression and replaces it with `| count`. And once again, the KISS principle prevails!


The bet was that I have to finish this in under 3 weeks, and I added these last finishing touches on the 3rd day. I told my boss he'd better pay me 7x the promised amount. He's yet to deliver. Up next was open-sourcing and distributing my beautiful work. To do that, I had to add tests, a nice README, and publish to NuGet. I did that over the weekend and on this fine Monday, I have just finished typing this really long account that nobody asked for!

I really wanted to do more of a tutorial-style post as a spiritual successor to Arthur Vickers' [*So you want to write an EF Core provider...*](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) which is what the official and outdated documentation recommends. I could be the one to finally update that old documentation, but that would take a little more effort, so it'll have to wait just a bit.