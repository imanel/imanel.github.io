---
layout: post
title: Web standards in mobile browsers
---

For the past few weeks I focused on developing [Socky](http://github.com/socky/socky-server-ruby) - new WebSocket based push server for Ruby on Rails. Its main advantage was to be working well in places where there is no flash - like iPhone or iPad. It is therefore obvious that I became very sensitive to standard implementation in so called "modern browsers" - also mobile ones. So, taking the opportunity of just released iOS4, I decided to write a few words about standard support in mobile browsers.

First tested was the latest iOS4 - loudly advertised as a strong HTML5 supporter. A set of rapid tests showed that the most important elements actually works flawlessly - audio and video, geolocation or SVG. Also, elements such as CSS 3 border-radius, CSS animations and reflections are working properly. But the most important feature for me, WebSockets, is missing. I don't understand why this protocol isn't there, when in lately released Safari 5 it wasn't issue. So how are users of the system be able to use chat, or HTML multiplayer games? There is also no support for @font-face or Web Workers. Maybe we see in the near future?

The second largest mobile system is Android - I tested version 2.2(not yet available everywhere). It's not disappointing - most of the tests passed without error. As in the iOS audio and video tags work properly, geolocation and CSS 3 too, and, moreover, does @font-face. Unfortunately, there also is no support for WebSocket (which Google Chrome has for a long time) - but fortunately, working flash eliminates this problem (thanks to [web-socket-js](http://github.com/gimite/web-socket-js)) I was surprised by the lack of SVG support - I understand that this is a very processor aggravating technology, but lack of it is very arduous.

Last discussed system is a WebOS version 1.4.1 (there should be new version 1.4.5 in the next few days) - system is based entirely on HTML and CSS, so you would expect the best support for standards. Unfortunately, there is some disappointment - most of the tests pass, but from presented systems this one have highest number of unsupported technology - @font-face, WebSockets, SVG, and geolocation. Hopefully the next update will improve browser a little.

All tests were performed using a [Modernizr](http://www.modernizr.com), and some of them tested manually (audio, video, WebSockets and geolocation)
