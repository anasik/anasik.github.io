---
title: I wrote an EF Core Provider
author: Anas Ismail Khan
layout: post
published: false
categories:
  - dotnet
  - csharp
  - Microsoft
  - aspnetcore
  - open source
---
The project I work on has a lot of data in a lot of entities with a lot of fields exposed through an OData API. Naturally, that compels us to know exactly what kind of query shapes our users may be interested in and to create optimal indexes for them beforehand. However, there's literally hundreds of fields and creating an index for every single one of them is simply not an option. Timeouts are great for protecting the database from maliciously demanding queries but they don't offer much in the realm of user-experience. We *need* to allow clients to send arbitrary queries and we *need* them to be fast regardless of whether we're expecting said query or not.

It was clear to me that we needed a solution and not an optimization and so I considered exploring Big Data solutions and columnar databases. Since our infrastructure is primarily azure based, my boss suggested that we explore Azure Data Explorer (ADX) aka Kusto. In a couple of days, ingestion was complete and a little testing showed promising performance. While we explored other options simultaneously, they're beyond the scope of this post. Kusto/ADX passed the test and we decided to explore our options for integration. 

Sadly, Kusto not only doesn't have an EF Core provider package, the T-SQL support, although well-marketed, was poorly performing. It was very clear to us that this path would require a little investment. After discovering that `EFCore.Snowflake`, another library/database pair we considered using, is developed and maintained by one person alone, I decided that I too will take a crack at writing a database provider. I mean, how hard could it be in today's AI powered era?

So on 18/11/2025, I started by carefully exploring the code for `EFCore.Snowflake`. I knew that my implementation would be far simpler since I didn't need to support design time services and OLTP flows. But at the same time, I also expected there to be complications surrounding the connection flow since Kusto doesn't support the ADO.NET architecture as the Kusto.Data library internally uses the REST API.

About 5 hours later, I could tell that it was functioning but I couldn't prove it because of a slight hiccup related to the handling of SQL Parameters. Basically, the final kql generated had parameter names in it and they had to be replaced with their respective values. I went to bed so I could approach this problem with a clear head in the morning.


