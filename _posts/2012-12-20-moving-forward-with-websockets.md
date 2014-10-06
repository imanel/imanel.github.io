---
title: Moving forward with WebSockets
redirect_to:
  - http://blog.imanel.org/moving-forward-with-websockets
---

Two years ago I created [LibWebSocket](https://rubygems.org/gems/libwebsocket) - gem designed to abstract complicated WebSocket API and make it easy to use. During this time a lot things happened - several big gems started using LibWebSocket(like Selenium-Webdriver and Pusher), and it was downloaded nearly 1,5 million times. A lot changed in WebSocket world itself - couple drafts passed, specification [was standardized](http://datatracker.ietf.org/doc/rfc6455/?include_text=1) and most browsers implemented [native support](http://caniuse.com/websockets) for WS protocol.

But time passes and old solutions needs updates. Unfortunately, because big changes in specification it's impossible to keep LibWebSocket up to date - mostly because of new features that are incompatible with some design decisions made during creation of LibWebSocket. Because of that I decided that best option is to rewrite it from scratch, with new standards in mind. As a result I would like to announce new gem: [WebSocket-Ruby](https://github.com/imanel/websocket-ruby).

Most of you will ask "why we need another WebSocket implementation?". That is great question, and let me answer it before moving forward. Currently we have multiple servers and maybe one or two clients for WebSocket. Each of them implements standard by itself, which results in multiple bugs and quirks - even most widely used EM-WebSocket have its own collection. In addition to that focusing on both server part and WebSocket API part make development a lot slower - I strongly believe that splitting responsibility to separate gems is best approach(for proof please see Rack and Thin separation).

In order to make sure that this implementation is reliable I created two additional gems - WebSocket [Client](http://github.com/imanel/websocket-eventmachine-client) and [Server](http://github.com/imanel/websocket-eventmachine-server). At the time of writing this article both of them have full [Autobahn](http://autobahn.ws) compatibility and best performance from all other Ruby implementations.

My next step is to start making pull requests for other major WebSocket clients and servers implementations - the more of them will use common implementation, the better it will be, and whole community will benefit. So if you are maintainer of such implementation, or you are willing to simply help transferring your favorite one to WebSocket-Ruby then I will gladly help.
