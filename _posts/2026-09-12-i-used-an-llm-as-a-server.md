---
title: I used an LLM as a Web Server
author: Anas Ismail Khan
layout: post
permalink: /i-used-an-llm-as-a-web-server/
categories:
  - AI
  - Tech
  - Software
  - Dev
  - Essays
  - open source
---
> I used an LLM as a web-server. My motivations for doing so are as unclear to myself as they are to you. This is just something I'd been wanting to experiment with for a while.
>
> It's currently live at [llmasaserver.pages.dev](https://llmasaserver.pages.dev/). The code is at [github.com/anasik/llm-as-a-server](https://github.com/anasik/llm-as-a-server).

## Prologue
Ever since I generated my first Gemini API key, I've had this crazy idea of making an LLM respond to HTTP requests while pretending to be a web-server and see how that goes. 

I immediately did a dry-run by opening a new Gemini chat and giving it explicit instructions to pretend to be a static website for a predetermined persona. 

It worked great, it sent me back HTML+CSS with menus, colors and well-crafted page sections. I could *send requests* to arbitrary pages like `/about` or `/contact` and it would play along. 

Then I took it a step further and *sent* `POST` and `PATCH` requests to imaginary resource endpoints and it responded with successes and failures depending on how relevant the endpoint was to the site content. 

It got even more real when I sent in `DELETE` requests followed by `GET` calls and sure enough, it was smart enough to not return any deleted objects.

The last test I performed was one where I told it to deny any requests that are missing a certain bearer token and just like that we had fake authorization.

Soon afterwards, however, I got a reality check as suddenly all context/memory started collapsing. The static pages changed completely in both design and content and subsequent API calls returned no data. A little probing led to Gemini apologizing to me for having a small context window and limited memory capabilities.

I didn't view this necessarily as a setback, however, because the whole point of this experiment was to have an LLM generate responses on the fly instead of caching them. Yes, consistency was both expected and desired but determinism was never on the table. I made a note to revisit this later and moved on with my work. <!--more-->

## Design
A few days ago, I rediscovered that note and decided to get to work. I started off by defining some bounds and parameters for the experiment. I wasn't quite sure what kind of utility a successful version of this would provide so I decided that the first version has to be built with absolutely free-to-use services. So in a way, free tier models and free hosting became part of the problem description.

I decided to use Groq or Gemini for the API and Cloudflare pages for hosting. The application was simple: a wrapper or a shell that forwards all incoming requests to an LLM model over an API in a prompt prefixed with the site identity and clear instructions to simulate a response in a certain format.

I decided that the identity of the site can be the experiment itself i.e. The website the LLM will pretend to be is a website about an LLM pretending to be a server. 

I further decided that the LLM should be able to request R/W access to some sort of sandboxed filesystem interface that it can use, but is not obligated to, to create any objects on behalf of the user or for caching purposes perhaps. This was mainly intended to serve the part where the LLM also pretends to be a REST API. 

The initial design ended up looking something like this: 
1. App receives request
2. App forwards request to LLM with the current filesystem snapshot.
3. LLM responds with the response and can request any CRUD ops on the filesystem.
4. App returns response and performs LLM filesystem actions.

## Implementation
All that sounds great in theory, but now I had to map it to an implementation. Up till then, I had mostly envisioned this as a PHP file reading a markdown file and calling an LLM API but since I had already made the brilliant decision to host this on Cloudflare, that already meant no persistent local filesystem and no PHP. 

The PHP gap was easy to fill with Cloudflare Functions. For the filesystem, I decided to build a tiny virtual filesystem around Cloudflare R2. 

Of course it wasn't actually possible to send the whole filesystem snapshot to the LLM every time without exhausting our limited tokens so I decided to use Cloudflare D1 to store session state. This is also where *references* to any created files will be tracked.

Since state was already per-session, the filesystem had to be too. Every visitor gets their own state document and their own private namespace in the VFS, so a file created on behalf of one is not merely hidden from another, it's unaddressable. 

Lastly, I decided to strip scripts, embeds and external assets from generated HTML, mostly for security reasons.

I mostly handed this design to Claude Code because that was the only way this experiment was ever gonna see the light of the day. One clear instruction I gave at that point was that there should be one markdown file at the root serving as the source of truth. This is the file that contains details about what this pseudo-server represents and how it behaves.

The first version was promising but it had one issue: the LLM was using the state document to cache entire rendered pages to provide a deterministic browsing experience. Since I particularly wanted visitors to experience that non-determinism, I explicitly forbade response caching. This desire for said non-determinism was also a secondary motivation for the session-scoped filesystem mentioned earlier.

However, this happy accident led to two key observations:
1. Caching pages inside state was the expensive option, not the cheap one: state goes back to the LLM on every later request, so a cached page keeps costing input tokens without saving model calls. 
2. Prompt caching matches as far as two prompts agree, so state that doesn't change unnecessarily can get cached along with the instructions in the prefix, leading to higher token efficiency.

My favourite finding though was the favicon. Every path goes to the LLM, and browsers ask for `/favicon.ico` on their own, so every page view was quietly costing two inferences instead of one. Half my rate limit was going to an icon nobody was looking at.

I could have just served a static favicon, but that felt like cheating given the purpose of this experiment. I decided that the LLM should generate the icon inline using a data URI. I initially added instructions for generating base64 but it kept coming back invalid. Eventually, I settled on percent-encoded SVG which worked every time. 

But even with the inlined favicon, almost every other request was `429`ing (too many requests). I thought of switching to a lower tier model but the limits were the same. That's when I had the genius idea of using a Gemini key as a fallback. And while I was at it, I thought why not also add an OpenRouter key as a fallback-fallback while we're at it. That improved uptime drastically.

Getting Gemini to behave still took a while. Before I can explain why, here's a little refresher about how LLM APIs work: you don’t send one blob of text. You send a list of messages, and each message carries a role. User messages represent the conversation, while system messages provide the instructions and context that govern how the model should respond.

I was sending two system messages, one holding the site definition and one holding the required response format. That worked well with Groq and OpenRouter but Gemini’s OpenAI-compatible endpoint appeared to discard the first message i.e. the site definition. So every request that fell through to Gemini returned perfectly well-formed JSON wrapping a hallucinated website. Once caught, fixing this was as simple as concatenating two strings into one. 

By this point, I was satisfied with the results of my experiment and decided to deploy it. As I was pushing the repo to GitHub, I wondered if in its current state it was even useful to anyone. Of course that begs a much more fundamental question of what purpose this repository even serves. But lets assume that its very existence defines its purpose, was it really in a customisable enough state to be a public repository?

Sure there was a markdown file containing the site description and anyone could edit that file and change it to whatever they want their gimmicky LLM-powered website to represent. Except, that file didn't just contain the site identity, it also contained the complete application layer between the lines. 

It wasn't a deal-breaker but it meant that one would have to surgically edit that file if they wanted to keep the current behaviour intact. So I decided to have Claude split it into two distinct markdown files: `CONTRACT.md` for the runtime rules and `SITE.md` for the site identity and behaviour.

And that was roughly where I stopped. I wanted an LLM to pretend to be a web-server and I did that. While it may be fun to explore this further and come up with more frameworks, I don't think I will necessarily continue down that path, as insightful as that would be.

## Epilogue
This is a demo and an experiment. It's very slow for two reasons:

1. When one model `429`s, it makes another round-trip to another model before responding.
2. Each generated response requires an inference and inferences can take several seconds. 

One thing I hadn't realised earlier because I'd been stingy about reading the documentation was that identical limits aren't necessarily shared ones. The Groq models I tested had separate buckets, which I only confirmed later by spamming `gpt-oss-120b` until it `429`'d then immediately switching to `gpt-oss-20b` and watching it respond. I was in the middle of writing this post when I realized I could also route within Groq before needing to fall back to Gemini or OpenRouter. This is what truly saved availability.

But that didn't just buy us availability. The server validates every response against the schema and rejects any deviations, which until now meant sending an error back to the user. However, with a fallback chain we can simply retry the request on a different model.