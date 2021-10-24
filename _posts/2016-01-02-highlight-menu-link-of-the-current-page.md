---
id: 1241
title: Highlight menu link of the current page.
date: 2016-01-02T14:50:41+00:00
author: Anas Ismail Khan
layout: post
guid: http://anasismail.com/?p=1241
permalink: /highlight-menu-link-of-the-current-page
categories:
  - Web and dev
tags:
  - :active
  - Highlight menu link of the current page
  - set element state to active
  - set menu link active
---
On some websites, regardless of whether or not they are wordpress or even PHP, the menu link for the page you are viewing gets _highlighted_ or it becomes _active. _An example could be the [2014 theme](https://wordpress.org/themes/twentyfifteen/) for WordPress (_<del>see how, in the nav bar at the top, the link for home is green?</del>_)_ _or [css-tricks.com](https://css-tricks.com/) (<del><em>link for the current page is orange.</em></del>)

In static web pages, there could be more than one ways of doing this, and I&#8217;m not even gonna go there. But in dynamic websites, where there could be tons of pages with the same elements loaded, like the nav, the header, e.t.c. you can&#8217;t really change the attributes of a certain list item to suit a particular page.  
Now the thing is, this isnt exactly rocket science.. Most good WordPress themes have this by default, but thanks to those few that don&#8217;t, I know that there&#8217;ll always be those who&#8217;d ditch a theme they spent ages styling on, simply because they&#8217;ll be tempted to use one that has it.

The day before I was modifying a theme, and I was almost completely done making it look like I wanted to, when I noticed that the css for **:active** state on the links didn&#8217;t apply. Of course that was stupid, no one really bothers with the active state anyway, they usually just define their own class.. After comparing the theme with another, and failing to find out how a general theme does it, I decided to code it in myself.

[CSS-Tricks has this age old tutorial video ](https://css-tricks.com/video-screencasts/36-current-nav-highlighting-using-php-to-set-the-body-id/) that shows a way to code it in yourself. Basically the idea is to parse the URL, and get the page request from the permalink. If you played the video, you can see how he got the page&#8217;s name and he applied it to the body as an id, and in his css he defined a style for body with ids matching the names of all his pages.

I however would do it slightly differently.. Just like he did, we get the request URL, get rid of slashes and question marsk (_if any,_) and apply it to a var ready to be echoed. But instead of applying and id to the body, and adding a comma separated &#8220;body#pagename&#8221; a number of times, we can define css for a particular class, like, for example _&#8220;.current,&#8221;_ and add a script to the page, which gets the page&#8217;s name from the url, and finds the menu item with that name in it&#8217;s _&#8220;title&#8221;_ attribute, and applies the _&#8220;.current&#8221;_ class to it.

So all you gotta do is add the style for the _.current _class to the css file and then add the php from that css-tricks tutorial (_the $page variable,_)  to the header.php. And then you need to add a small Javascript _script_, that takes that $page var (don&#8217;t underestimate the power of `<?php echo $page; ?>`,) and applies the class to the corresponding menu link.

As a bonus, I will add below all the code you need to make this work.

<?php

$page = $\_SERVER[&#8216;REQUEST\_URI&#8217;];  
$page = str_replace(&#8220;?&#8221;,&#8221;&#8221;,$page);  
$page = str_replace(&#8220;/&#8221;,&#8221;&#8221;,$page);  
$page = str_replace(&#8220;.php&#8221;,&#8221;&#8221;,$page);  
$page = $page ? $page : &#8220;home&#8221;; ?>

Add this PHP at the very top of the header file.. before _everything, _and then, anywhere below below the nav markup, add this JS script:

<script>  
nav = document.getElementsByClassName(&#8220;nav&#8221;)[0];  
nav = nav.getElementsByTagName(&#8220;a&#8221;);  
for(i=0;i<nav.length;i++){  
if(nav[i].getAttribute(&#8220;title&#8221;).toLowerCase() == &#8220;<?php echo strtolower($page);?>&#8221;){  
nav[i].setAttribute(&#8220;class&#8221;,&#8221;current&#8221;);  
}}  
</script>

You might have noticed that this only works if the container of the nav has a &#8220;_.nav_&#8220;_ _class to it. Even if it doesn&#8217;t that can simply be added, and if there&#8217;s a different class name you are inclined to use, then simply use it, or if there&#8217;s an id, replace the &#8220;_getElementsByClassName_&#8221; with_  &#8220;getElementById.&#8221; _But that wasn&#8217;t just it. Another prerequisite for it to work is for the _a _tags in the nav to have _title _attributes. So yeah maybe I&#8217;m just wasting my time posting this. But _hey! _Whatever works, works.