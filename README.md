# CHAT REAL TIME

![chat](https://user-images.githubusercontent.com/60434681/82680245-5eb59100-9c22-11ea-8951-42520e1e9c6f.gif)


- Criar aplicações real-time com NodeJS fica muito fácil utilizando o poder do Socket.io.
Aplicação de chat em tempo real conversando com um servidor em NodeJS.

## SOCKET  O que é 
- Socket.IO é uma biblioteca JavaScript para aplicativos da web em tempo real. Permite comunicação bidirecional em tempo real entre clientes e servidores da Web. Ele tem duas partes: uma biblioteca do lado do cliente que é executada no navegador e uma biblioteca do lado do servidor para o Node.js.

## Server code
```
const express = require('express');
const path = require('path');

const app = express();
const server = require('http').createServer(app);
const io = require('socket.io')(server);

app.use(express.static(path.join(__dirname, 'public')));
app.set('views', path.join(__dirname, 'public'));
app.engine('html', require('ejs').renderFile);
app.set('view engine', 'html');


app.use('/', (req, res) => {
  res.render('index.html')
});

let messages = [];

io.on('connection', socket => {
  console.log(`Socket conectado: ${socket.id}`)

  socket.emit('previousMessage', messages)

  socket.on('sendMessage', data => {
    messages.push(data);
    socket.broadcast.emit('receivedMessage', data);
  });
})

server.listen(3000);
```

## Frontend Code
```
 <script type="text/javascript">
    var socket = io('http://localhost:3000');

    function renderMessage(message) {
      $('.messages')
      .append('<div class="message"><strong>'+ message.author +'</strong>: '+ message.message +'</div>');
    }

    socket.on('previousMessage', function(messages) {
      for (message of messages){
        renderMessage(message)
      }
    })

    socket.on('receivedMessage', function(message) {
      renderMessage(message);
    });

    $('#chat').submit(function(event) {
      event.preventDefault();

      var author = $('input[name=username]').val();
      var message = $('input[name=message]').val();

      if (author.length && message.length) {
        var messageObject = {
          author: author,
          message: message,
        };

        renderMessage(messageObject);

        socket.emit('sendMessage', messageObject);
      }
    })
  </script>
```

## Dependencias
```
# yarn add els express socket.io 
```

