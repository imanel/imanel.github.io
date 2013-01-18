---
layout: post
title: Building WebSocket server in Ruby
comments: true
---

[Vti's post](http://showmetheco.de/articles/2010/11/timtow-to-build-a-websocket-server-in-perl.html) about building WebSocket server inspired me to write another one, but this time about both server and client in Ruby.

Currently we have couple of great implementations of WebSocket in Ruby - [em-websocket](http://github.com/igrigorik/em-websocket), [web-socket-ruby](http://github.com/gimite/web-socket-ruby) or [sunshowers](http://rainbows.rubyforge.org/sunshowers) to name a few. And looking at each one of them I can see two things - they all are great, and they all are alone. What I mean?

[Github](http://github.com) alone says that there are 40 projects in Ruby connected to WebSocket. Some of them are basing one on another, but there are more than 15 server implementations from scratch. Only two support both Draft 75 and Draft 76. And only one support newer drafts. Am I only one who see the problem?

That's why I built [LibWebSocket](http://github.com/imanel/libwebsocket) library whose main purpose is to eliminate the problem of aging server implementations - wrapper around multiple drafts of WebSocket with common API that any server implementation can build against. Thank's to that whole community will be able to gain instead of working independently. So how to get started?

## Building server

Currently most popular Ruby WebSocket server is em-websocket(I'm also using it in my [Socky](http://github.com/socky) project) and I believe that currently it's the best implementation. So why don't start from building similar server which just couple of lines instead of huge library?

``` ruby
#!/usr/bin/env ruby

require 'rubygems'
require 'eventmachine'
require 'libwebsocket'

module EchoServer
  def receive_data(data)
    @hs ||= LibWebSocket::OpeningHandshake::Server.new
    @frame ||= LibWebSocket::Frame.new

    if !@hs.done?
      @hs.parse(data)

      if @hs.done?
        send_data(@hs.to_s)
      end

      return
    end

    @frame.append(data)

    while message = @frame.next
      send_data @frame.new(message).to_s
    end
  end

end

EventMachine::run do
  EventMachine::start_server '0.0.0.0', 8080, EchoServer
  puts "Started EchoServer on #{host}:#{port}..."
end
```

And that's all! Of course it's just simplified implementation, but I think that shows what I'm talking about. No more handling events, checking version and implementing multiple drafts - it's all inside! And together with that we can also build client:

## Building client

This one is more tricky but still most work is handled by LibWebsocket:

``` ruby
require "socket"
require 'libwebsocket'

class WebSocket

  def initialize(url, params = {})
    @hs ||= LibWebSocket::OpeningHandshake::Client.new(:url => url,
        :version => params[:version])
    @frame ||= LibWebSocket::Frame.new

    @socket = TCPSocket.new(@hs.url.host, @hs.url.port || 80)

    @socket.write(@hs.to_s)
    @socket.flush

    loop do
      data = @socket.getc
      next if data.nil?

      result = @hs.parse(data.chr)
      raise @hs.error unless result

      if @hs.done?
        @handshaked = true
        break
      end
    end
  end

  def send(data)
    raise "no handshake!" unless @handshaked

    data = @frame.new(data).to_s
    @socket.write data
    @socket.flush
  end

  def receive
    raise "no handshake!" unless @handshaked

    data = @socket.gets("\xff")
    @frame.append(data)

    messages = []
    while message = @frame.next
      messages << message
    end
    messages
  end

end
```

This will allow to connecting, sending and receiving messages from WebSocket server in drafts #75 and #76(more vesions comming)

## State and future

[LibWebSocket](http://github.com/imanel/libwebsocket) is currently in very early state of development but I hope that it will move fast to stable version. You can now install it as gem and start using:

``` sh
gem install libwebsocket
```

See project at github: [http://github.com/imanel/libwebsocket](http://github.com/imanel/libwebsocket)
