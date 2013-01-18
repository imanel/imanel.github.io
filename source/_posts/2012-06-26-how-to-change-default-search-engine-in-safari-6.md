---
layout: post
title: How to change default search engine in Safari 6?
comments: true
---

When new Safari will be released plugins like [Safari Omnibar](http://hackemist.com/SafariOmnibar/) will be obsolete - new Safari has functionality similar to Chrome Omnibox. Yet - one function is missing: option to change search provider. I'm pretty sure that a lot of us use plugins like SafariOmnibar not just to integrate Google Search with URL bar, but also to be able to use different search providers than Google, Bing and Yahoo.

Good news: it is still be possible to use custom search. Bad news: it's little different that earlier.

If you want to integrate multiple search engines like Wiki, Youtube, Quora and many more, then I would suggest to use to [DuckDuckGo](https://duckduckgo.com) and it's awesome [!Bang syntax](https://duckduckgo.com/bang.html) - not only it will allow accessing multiple search engines, but will provide much more functionality than standard Google.

## How to change provider to DuckDuckGo?

I know that there's a lot of scary methods around net that will show you methods like manualy editing binary file or messing with configuration. I'm not a big fan of those - mostly because after every Safari update you will be forced to do it again. What I prefer is much cleaner way of [editing single text file in system](http://help.duckduckgo.com/customer/portal/articles/255650).

What you will need to do is editing file /etc/hosts, which contain DNS entries, and change one of currently used search providers to DuckDuckGo server.

``` sh
184.72.115.86 search.yahoo.com
```

This will change all searches via "Search using Yahoo!" option to search via DuckDuckGo. It's possible to change Google or Bing search to work like that, but it's not recommended because it will prevent to access all Google/Bing functionality, when Yahoo have it's search engine in separate subdomain so it's non-obtrusive.

## But I want to still use Google as main engine!

For some of you fully changing to DuckDuckGo is no-go. Because of that there's another method - redirecting all Google Searches with !Bang syntax to DuckDuckGo(which will redirect to according search engine or page).

In order to do so you will need to support for Greasemonkey UserScripts. There was obsolete method with SIMBL plugin for that, but currently Safari extension build on standard APIs exists - it's called [NinjaKit](http://mac.softpedia.com/get/Internet-Utilities/NinjaKit-for-Safari.shtml). Please install it to gain access to all user scripts available at [http://userscripts.org/](http://userscripts.org/).

After installing NinjaKit all you need is to add simple UserScript that will detect and redirect queries staring with bang - you can find it [here](https://gist.github.com/1817223). From now on you can use normal Google Search, but when i.e. want to search YouTube instead just type in search bar:

``` sh
!yt funny cats
```
