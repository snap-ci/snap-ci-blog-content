---
layout: post
title:  "Playing with WebSockets for Fun and Profit: Debugging Remote Machines"
description: "The web has come far from the days of AJAX polling to where the browser constantly polled the server for updates. In general there are a few ways to get a message from a server to a browser, each of these has their benefits and drawbacks."
date:   2015-10-28
author: Ketan Padegaonkar
index-image-url: screenshots/websockets/websockets.png
index-image-alt: 'Playing with WebSockets'
keywords: websockets, debugging, ajax, debugger, snap shell
categories: WebSockets snap-shell
---

The web has come far from the days of AJAX polling to where the browser constantly polled the server for updates. In general there are a few ways to get a message from a server to a browser, each of these has their benefits and drawbacks.

* Constant reloading of the entire page every few seconds.
* **AJAX** - aka constant fetching of an HTML/JSON payload. This is usually used together with some DOM tricks in the browser to make it look like the page is not reloading. This has the downside that every poll has a high cost of setting up and tearing down HTTP
* **Long polling** - the server keeps a connection open for long periods of time, and sends back messages when something interesting happens.
* **Server-Sent Events** (HTML5 EventSource) - this is a form of long polling where the server sends back data when it has any. The client does nothing but wait for messages. All messaging is one way from the server to the client.
* **WebSockets** - This is a true bi-directional real-time communication mechanism available to the browser.

WebSockets present exciting possibilities but also are very different to the way the web works. They cause problems in how they interact with proxies and load-balancers or how and also in the level of support they have in common web-servers like Apache. These problems notwithstanding, they are exciting because of the possibilities they offer. However, common examples of WebSocket usages show how to build a chat client – which is not a particularly exciting way to showcase why they are exciting. So in this blog, I am going to discuss an interesting idea and explore how to build something more fun with WebSockets.

## The Problem Description

We would like to build something that would offer near real time command line like behavior in the browser. When a user types in a command in a browser, the response must come back as soon as possible. The command that is typed in by the user could be either something that is expected to finish immediately or take a fair bit of time, and therefore, it is hard to say beforehand how frequently we should poll for the output. If we want swift response, we might choose to poll every 500ms and therefore risk hammering the server, especially in a multi-user scenario. If we picked a slower poll time, we risk providing users a really sluggish experience.

## An Improved Alternative

What we want is for the UI to update exactly when there is some output, no sooner or later. This requires that the server inform the browser as soon as it has some output it would like to present to users. This is exactly the sort of thing that SSE and WebSockets are great at. In this example, we will look at how we can use [Sinatra](http://www.sinatrarb.com/) with a WebSocket library, [Faye](https://github.com/faye/faye-websocket-ruby), to implement this.

## The Protocol

To relay information and state between the debug client and the various browsers that may be connected, we choose a JSON protocol.

When a command is submitted by a browser, there are two things that could happen:

1. The debugger is already running a command, so it rejects the new one.
2. The debugger is not busy, it accepts the command and acknowledges that it's begun executing it.

When the debugger is done executing its command, it should notify the browser that it's ready to accept new commands.

![websockets](/assets/images/screenshots/websockets/websockets.png){: .screenshot .big}

## The Implementation

Let us start with a minimal WebSocket server application. The server is a Sinatra application that accepts the WebSocket request. It uses a helper Debugger model to keep track of connections and execute commands.

Once the request is accepted, there are 3 callbacks, that are available with the WebSocket open, close and message. These are invoked when a new connection is made, closed or if data is sent by the browser to this server.

The command would be processed using our debugger model instance. This model can take a few callbacks, before_execute and after_execute, before and after it processes any commands.

<pre class="brush:ruby prettyprint"># sinatra server
get '/debug' do
 if Faye::WebSocket.websocket?(request.env)
   handle_browser_connection
 else
   # tell the client to upgrade to websocket
 end
end

def handle_browser_connection
 websocket = Faye::WebSocket.new(request.env, nil, ping: 5)

 debugger = Debugger.instance

 debugger.before_execute do
 end

 debugger.after_execute do
 end

 websocket.on :open do |event|
 end

 websocket.on :message do |event|
 end

 websocket.on :close do |event|
 end
 websocket.rack_response
end</pre>

The javascript client in the browser creates a WebSocket, this WebSocket has callbacks that are identical to those on the server.

<pre class="brush:ruby prettyprint">&lt;input id='inputBox' /&gt;
&lt;script&gt;
var websocket = new WebSocket("wss://example.com/debug");
var inputBox = $('#inputBox');

websocket.onopen = function(event) {
};

websocket.onmessage = function(event) {
};

websocket.onclose = function(event) {
};

inputBox.keydown(function(event){
});

</pre>

With the basic structure in place on the server and browser, we can move on to building pieces of our protocol. Let us start with the javascript where users enter a command.

<pre class="brush:ruby prettyprint">inputBox.keydown(function(event){
   // trap the enter key and send the command over the websocket
  if (event.keyCode == 13){
    event.preventDefault();
    var inputCommand = inputBox.val();
    websocket.send(JSON.stringify({command: inputCommand}));
  }
 });
</pre>

When the server receives an open from a browser, it remembers that browser connection. When a message is received by our server, it asks the debugger to execute the command if it is not already busy.

The debugger, before it executes any command, should tell all browser clients that it has accepted the command, and is now busy processing it.

Once the debugger has finished the command, it should tell all browser clients that it is no longer busy.

<pre class="brush:ruby prettyprint">#sinatra server
 debugger.before_execute do
   # tell all browsers that we are busy
   debugger.browser_clients.each |websocket_to_browser|
     websocket_to_browser.send({accepted: true, busy: true}.to_json)
   end
 end

 debugger.after_execute do
   # tell all browsers that we are not busy anymore
   debugger.browser_clients.each |websocket_to_browser|
     websocket_to_browser.send({busy: false}.to_json)
   end
 end

 websocket.on :open do |event|
   # remember that this browser is a participant, and let it know if we're busy
   debugger.browser_clients &lt;&lt; websocket
   websocket.send({busy: debugger.busy?}.to_json)
 end

 websocket.on :message do |event|
   message = JSON.parse(event.data)
   command = message['command']
   debugger.execute(command) unless debugger.busy?
 end</pre>

Back to our javascript - it now needs to process these messages to provide some feedback in the browser.

<pre class="brush:ruby prettyprint">websocket.onmessage = function(event) {
 var msg = JSON.parse(event.data);
 if (msg.busy) {
   inputBox.attr('disabled', 'disabled');
 } else {
   inputBox.removeAttr('disabled');
 }
}</pre>

This is a simple example that shows some of the interesting applications one could build using modern browsers and WebSockets. I would love to hear about some of the interesting things that you are doing with WebSockets!

To see how this simple example could work when the rough edges have been smoothed out, check out Snap's [snap-shell](http://blog.snap-ci.com/blog/2014/08/11/introducing-snap-shell/) feature.

<span class = "italic-text">This post was originally published on [ThoughtWorks Insights](https://www.thoughtworks.com/insights/blog/playing-websockets-fun-and-profit-debugging-remote-machine) Technology channel.</span>
