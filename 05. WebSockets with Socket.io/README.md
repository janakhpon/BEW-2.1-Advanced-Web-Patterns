# Adding WebSockets with Socket.io

## Competencies & Objectives

1. Identify the use cases for WebSockets
1. Explain the difference between HTTP and WebSocket standards and communication
1. Implement the simplest possible WebSocket event-driven communication in our spec project.

## Intro

So far in our exploration of web patterns we've always used HTTP to make synchronous, call-and-response type requests to servers. But what happens when you want clients and servers to communicate asynchronously as in a chat application or with live updates or streaming information. As of a few years ago, a powerful new standard called *WebSockets*.

In the following tutorial we'll be using [Socket.io](https://socket.io/) one of the most reliable npm modules on the web that enables node servers to respond to event-driven WebSocket behaviors. Socket.io is a broad and powerful library that can manage multiple channels, rooms, and manage https and other forms of security. In our case we will be implementing a simple asynchronous push of data from the server to the client.

Remember that since the client and the server are communicating via WebSockets, Socket.io will have a client- and server-side library you will need to initialize in your project. A simple example is as follows:

```html
<!-- CLIENT -->
<script src="/socket.io/socket.io.js"></script>
<script>
  var socket = io('http://localhost');

  socket.emit('ferret', 'tobi', function (data) {
    console.log(data); // data will be 'woot'
  });
</script>
```

```js
// SERVER
var io = require('socket.io-client')('http://localhost');
io.on('connection', function (socket) {
  socket.on('ferret', function (name, fn) {
    fn('woot');
  });
});
```

Hint: If you are starting your server you need to include io in that server.

> **NOTE** - `broadcast.emit()` and `emit()` are different. `emit()` sends to all attached sockets, `broadcast.emit()` sends to all except the one that is sending the message.

```js
// SERVER
var app = require('express')();
var http = require('http').Server(app);
var io = require('socket.io')(http);

app.get('/', function(req, res){
  res.sendFile(__dirname + '/index.html');
});

io.on('connection', function(socket){
  console.log('a user connected');
});

http.listen(3000, function(){
  console.log('listening on *:3000');
});
```

## Resources

1. [WebSocket Example](http://codepen.io/voku/pen/GpVoNN?editors=1010)
1. [REST vs. WebSockets (PubNub)](https://www.pubnub.com/blog/2015-01-05-websockets-vs-rest-api-understanding-the-difference/)
1. [Introducing WebSockets: Bringing Sockets to the Web](https://www.html5rocks.com/en/tutorials/websockets/basics/)
1. [What are WebSockets (Pusher)](https://pusher.com/websockets)
