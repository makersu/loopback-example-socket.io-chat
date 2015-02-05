# loopbackchat
StrongLoop Loopback with socket.io chat example

#  Install StrongLoop
```
npm install -g strongloop
```

## Create new loopback application
```
slc loopback
```

## Integrating Socket.IO
```
npm install --save socket.io
```

## Defining middleware(server/middleware.json)
```
...
"routes": {
  },
  "files": {
    "loopback#static": {
      "params": "$!../client"
    }
  },
...
```

## Add a static web page(client/index.html)
```
<!doctype html>
<html>
  <head>
    <title>Socket.IO chat</title>
    <style>
      * { margin: 0; padding: 0; box-sizing: border-box; }
      body { font: 13px Helvetica, Arial; }
      form { background: #000; padding: 3px; position: fixed; bottom: 0; width: 100%; }
      form input { border: 0; padding: 10px; width: 90%; margin-right: .5%; }
      form button { width: 9%; background: rgb(130, 224, 255); border: none; padding: 10px; }
      #messages { list-style-type: none; margin: 0; padding: 0; }
      #messages li { padding: 5px 10px; }
      #messages li:nth-child(odd) { background: #eee; }
    </style>
  </head>
  <body>
    <ul id="messages"></ul>
    <form action="">
      <input id="m" autocomplete="off" /><button>Send</button>
    </form>
    <script src="https://cdn.socket.io/socket.io-1.2.0.js"></script>
    <script src="http://code.jquery.com/jquery-1.11.1.js"></script>
    <script>
    var socket = io();
    $('form').submit(function(){
      socket.emit('chat message', $('#m').val());
      $('#m').val('');
      return false;
    });
    socket.on('chat message', function(msg){
      $('#messages').append($('<li>').text(msg));
    });
    </script>
  </body>
</html>
```

## create chat server(server/server.js)
```
...
if (require.main === module) {
  //app.start();
  app.io = require('socket.io')(app.start());
  app.io.on('connection', function(socket){
  	console.log('a user connected');
  	socket.on('chat message', function(msg){
    	console.log('message: ' + msg);
    	app.io.emit('chat message', msg);
  	});
  	socket.on('disconnect', function(){
  		console.log('user disconnected');
  	});
  });
  
}
```

### run loopbackchat
```
slc run
http://0.0.0.0:3000/
```

## Reference
- Getting started with LoopBack(http://docs.strongloop.com/display/public/LB/Getting+started+with+LoopBack)
- Defining Express middleware(http://docs.strongloop.com/display/public/LB/Defining+middleware)
- Socket.IO(http://socket.io/get-started/chat/)

## Next
- create chat module(/server/boot/chat.js)
- login auth(http://docs.strongloop.com/display/public/LB/Authentication+and+authorization)
- Third-party login (Passport)(http://docs.strongloop.com/pages/releaseview.action?pageId=3836277)
- MongoDB connector(http://docs.strongloop.com/display/public/LB/MongoDB+connector)
- Storage service(http://docs.strongloop.com/display/public/LB/Storage+service)
- Redis(http://redis.io/)
- RabbitMQ(http://www.rabbitmq.com/)
- elasticsearch(http://www.elasticsearch.org/)
- webRTC(http://www.webrtc.org/)
- cordova(http://cordova.apache.org/)
- ionicframework(http://ionicframework.com/)



