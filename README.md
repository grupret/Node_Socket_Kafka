# Node_Socket_Rooms_Redis_PubSub_Kafka

# Steps:
npm init
npm install express

      const express = require('express')
      
      const app = express();
      app.get('/', (req, res) => {
          res.send('Hello Connection')
      })
      
      const server = app.listen(1337, () => {
          console.log("server running on port 1337")
      })

node app.js


      
# Socket.io
    *app.js*
                  
                  const express = require('express')
                  const socketio = require('socket.io')
                  
                  const app = express();
                  
                  app.get('/', (req, res) => {
                      res.send('Hello Connection')
                  })
                  
                  const server = app.listen(1337, () => {
                      console.log("server running on port 1337")
                  })
                  
                  const io = socketio(server);
                  
                  io.on('connection',(socket) =>{
                      console.log('New Connection')
                  })


npm install socket.io-client

     *app_client.js*



                  var io = require('socket.io-client');
                  
                  
                  mysocket = io.connect('http://localhost:1337');
                  
                  
                  mysocket.on('connect', function(){
                      console.log('connected!!')
                  })
     
                  
# 2 way communication

                              const express = require('express')
                  const socketio = require('socket.io')
                  
                  const app = express();
                  
                  app.get('/', (req, res) => {
                      res.send('Hello Connection')
                  })
                  
                  const server = app.listen(1337, () => {
                      console.log("server running on port 1337")
                  })
                  
                  const io = socketio(server);
                  
                  io.on('connection',(socket) =>{
                      console.log('New Connection');
                  
                  socket.emit('notification',"Thanks for connecting!!!")
                  
                      socket.on('message', (data) =>{
                          console.log("New message from client", data);
                      })
                  
                  
                  
                  
                  
                  })

 client

                   var io = require('socket.io-client');
                  
                  
                  mysocket = io.connect('http://localhost:1337');
                  
                  
                  mysocket.on('connect', function(){
                      console.log('connected!!')
                  })
                  
                  mysocket.on('notification', (notification) =>{
                      console.log("Message recieved from Server", notification);
                  })
                  
                  mysocket.emit('message',"Hey How are you ?");

#  rooms  subscriber
https://socket.io/docs/v3/rooms/

                            const express = require('express')
                            const socketio = require('socket.io')
                            const { createClient } = require('redis')
                            const { createAdapter } = require('@socket.io/redis-adapter')
                        
                        
                            const app = express();
                        
                        
                            const server = app.listen(1337, () => {
                                console.log("server running on port 1337")
                            })
                        
                            const io = socketio(server);
                        
                            io.on('connection',(socket) =>{
                                console.log('New Connection');
                        
                                    socket.on("newConnection", function(id, room){
                        
                                        socket.emit('notification',"Thanks for connecting!!!")
                        
                                        console.log(id, room);
                        
                                        if(room == 'ActionLogRoom'){
                                            console.log("=========================")
                                        socket.join('ActionLogRoom');    
                                        }
                                    
                                        io.to("ActionLogRoom").emit('notification',"Thanks for connecting to ActionLogRoom!!!");
                                    
                                        socket.on('message', (data) =>{
                                            console.log("New message from client", data);
                                        })
                                    
                                    
                                    })
                        
                            
                            })
                        
                        

# client 1
                  
                  var io = require('socket.io-client');
                  
                  
                  mysocket = io.connect('http://localhost:1337');
                  
                  
                  mysocket.on('connect', function(){
                      console.log('connected!!')
                  })
                  
                  
                  mysocket.emit('newConnection', 111, 'ActionLogRoom');
                  
                  mysocket.on('notification', (notification) =>{
                      console.log("Message recieved from Server", notification);
                  })
                  
                  mysocket.emit('message',"Hey How are you ?");
                  
                  


# client 2

                        
                        var io = require('socket.io-client');
                        
                        
                        mysocket = io.connect('http://localhost:1337');
                        
                        
                        mysocket.on('connect', function(){
                            console.log('connected!!')
                        })
                        
                        
                        mysocket.emit('newConnection', 111, 'GroupLogRoom');
                        
                        mysocket.on('notification', (notification) =>{
                            console.log("Message recieved from Server", notification);
                        })
                        
                        mysocket.emit('message',"Hey How are you ?");
                        
                        

                  



# Redis pub sub code:

*Redis is used for hpa of sockets so that id multi instance of socket are running no client connecting across the instances to the same rooms don't miss the messages...
https://github.com/sayar/RedisMVA/blob/master/module6_redis_pubsub/README.md
*                 
                  const express = require('express')
                  const socketio = require('socket.io')
                  const http = require('http')
                  const { createClient } = require('redis')
                  const { createAdapter } = require('@socket.io/redis-adapter')
                  
                  
                  const app = express();
                  const server = http.createServer(app)
                  const io = socketio(server);
                  
                  var pub = createClient({host: 'localhost', port:6379});
                  var sub = pub.duplicate();
                  
                  
                  Promise.all([pub.connect(), sub.connect()]).then(() => {
                  	io.adapter(createAdapter(pub, sub))
                  	io.listen(1337)
                  });
                  
                  
                  io.on('connection',(socket) =>{
                      console.log('New Connection');
                  
                          socket.on("newConnection", function(id, room){
                  
                              socket.emit('notification',"Thanks for connecting!!!")
                  
                              if(room === 'ActionLogRoom'){
                              socket.join('ActionLogRoom');    
                              }
                          
                              socket.to("ActionLogRoom").emit('notification',"Thanks for connecting to ActionLogRoom!!!")
                          
                              socket.on('message', (data) =>{
                                  console.log("New message from client", data);
                              })
                          
                          
                          })
                  
                     
                  })



                  
                  

                  



