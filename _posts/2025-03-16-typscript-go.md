---
title: Why did Anders Hejlsberg choose Go for Typescript instead of C# or Rust?
author: Anas Ismail Khan
layout: post
permalink: /why-did-anders-hejlsberg-choose-go-for-typescript-instead-of-csharp-or-rust?/
categories:
  - Uncategorized
tags: 
  - Anders Hejlsberg
  - Ryan Cavanaugh
  - Typescript
  - Golang
  - Rust
  - Zig
  - Corsa
  - Strada
  - Microsoft
  - C#
  - low-level language
  - compiler
  - transpiler
---
Ever since Anders Hejlsberg made the announcement for Project Corsa, aka Strada, aka typescript-go, there's been a lot of unrest surrounding their decision to use Go. 

On one hand, we have the Rust fans demanding to know why Rust wasn't chosen for this, considering the popularity of Rust as the de facto modern low-level language.

The dev lead of Typescript, Ryan Cavanaugh himself, took to Reddit  to address the first question[^1]. He said, and I quote: 

*"In the end we had two options - do a complete from-scrach rewrite in Rust, which could take years and yield an incompatible version of TypeScript that no one could actually use, or just do a port in Go and get something usable in a year or so and have something that's extremely compatible in terms of semantics and extremely competitive in terms of performance."*

On the other hand we have the C# community asking the real questions: Why did the lead architect of C# sit by such a decision? This one, was answered by Anders Hejlsberg himself in an interview with Michigan TypeScript[^2]: 

*"C# was considered but Go is definitely the lowest-level language we can get to and still have automatic garbage-collection [...] C# is byte-code first and while it has some AOT, it's not available on all platforms and it doesn't have decades of hardening and wasn't engineered that way to begin with. Go has a little more expressiveness when it comes to data-structures and inline structs. Our JS codebase is written in a highly functional style with very few classes which is also a characteristic of Go [...] we would have had to switch to an OOP paradigm to switch to C# [...] ultimately that was the path of least resistance."*

[^1]: Link to Ryan's reddit comment: https://lnkd.in/dqHx6gj3
[^2]: Link to Anders' interview: https://lnkd.in/dUJiwEzv