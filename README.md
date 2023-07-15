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


      
