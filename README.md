# Node_Socket_Kafka

Steps:
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


      
*Socket.io*
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
     
                  



