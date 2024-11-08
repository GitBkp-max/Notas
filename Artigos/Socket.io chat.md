---
tags:
  - Programação/Node
  - Programação/JS
  - Programação/socketio
  - Artigos
---
# Começar

Neste guia, vamos criar um aplicativo de bate-papo básico. Não requer quase nenhum conhecimento prévio básico de `Node.JS` ou ``Socket.IO``, por isso é ideal para usuários de todos os níveis de conhecimento.

## Introdução

Escrever um aplicativo de bate-papo com pilhas populares de aplicativos da web, como o LAMP (PHP), normalmente tem sido muito difícil. Envolve pesquisar o servidor para alterações, manter o controle de ``timestamps`` e ``it“s`` muito mais lento do que deveria ser.

Os soquetes têm sido tradicionalmente a solução em torno da qual a maioria dos sistemas de bate-papo em tempo real é arquitetada, fornecendo um canal de comunicação bidirecional entre um cliente e um servidor.

Isso significa que o servidor pode _empurrar_ mensagens para clientes. Sempre que você escreve uma mensagem de bate-papo, a ideia é que o servidor irá obtê-lo e enviá-lo para todos os outros clientes conectados.

## O framework web

O primeiro objetivo é configurar uma página HTML simples que sirva um formulário e uma lista de mensagens. Weilitre vai usar o framework web Node.JS `express` para esse fim. Certifique-se [Node.JS](https://nodejs.org/) está instalado.

Primeiro vamos criar um `package.json` arquivo de manifesto que descreve nosso projeto. Eu recomendo que você colocá-lo em um diretório vazio dedicado (Vou chamar o meu `chat-example`).

```json
{  
	"name": "socket-chat-example",  
	"version": "0.0.1",  
	"description": "my first socket.io app",  
	"dependencies": {}
}
```

> [!caution]
> A propriedade "name" deve ser única, você não pode usar um valor como "socket.io" ou "express", porque o npm reclamará ao instalar a dependência.

Agora, a fim de preencher facilmente o `dependencies` propriedade com as coisas que precisamos, Vamos usar `npm install`:

```node
npm install express@4
```

Uma vez instalado, podemos criar um `index.js` arquivo que irá configurar o nosso aplicativo.

```js
const express = require('express');
const app = express();
const http = require('http');
const server = http.createServer(app);

app.get('/', (req, res) => {  
	res.send('<h1>Hello world</h1>');
});

server.listen(3000, () => {  
	console.log('listening on *:3000');
});
```

Isso significa que:

- Inicializações expressas `app` para ser um manipulador de funções que você pode fornecer a um servidor HTTP (como visto na linha 4).
- Definimos um manipulador de rotas `/` isso é chamado quando chegamos em nosso site.
- Fazemos o servidor http ouvir na porta 3000.

Se você correr `node index.js` você deve ver o seguinte:

![[nodechat-example node index.jslistening on  3000.webp]]

E se você apontar seu navegador para `http://localhost:3000`:

![[Um navegador exibindo um grande Hello World.webp]]
## Servindo HTML[](https://socket.io/get-started/chat#serving-html "Link direto para Servir HTML")

Até agora em `index.js` estamos ligando `res.send` e passando-lhe uma cadeia de HTML. Nosso código pareceria muito confuso se colocássemos todo o nosso HTML de aplicações lá, então, em vez disso, vamos criar um `index.html` arquive e sirva isso em vez disso.

Letilits refatora nosso manipulador de rotas para usar `sendFile` em vez disso.

```
app.get('/', (req, res) => {  res.sendFile(__dirname + '/index.html');});
```

Coloque o seguinte em seu `index.html` ficheiro:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Socket.IO chat</title>
    <style>
      body { margin: 0; padding-bottom: 3rem; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; }

      #form { background: rgba(0, 0, 0, 0.15); padding: 0.25rem; position: fixed; bottom: 0; left: 0; right: 0; display: flex; height: 3rem; box-sizing: border-box; backdrop-filter: blur(10px); }
      #input { border: none; padding: 0 1rem; flex-grow: 1; border-radius: 2rem; margin: 0.25rem; }
      #input:focus { outline: none; }
      #form > button { background: #333; border: none; padding: 0 1rem; margin: 0.25rem; border-radius: 3px; outline: none; color: #fff; }

      #messages { list-style-type: none; margin: 0; padding: 0; }
      #messages > li { padding: 0.5rem 1rem; }
      #messages > li:nth-child(odd) { background: #efefef; }
    </style>
  </head>
  <body>
    <ul id="messages"></ul>
    <form id="form" action="">
      <input id="input" autocomplete="off" /><button>Send</button>
    </form>
  </body>
</html>
```

Se reiniciar o processo (através do Control+C e em execução `node index.js` novamente) e atualize a página deve ficar assim:

![[Um navegador exibindo uma entrada e um botão 'Enviar.webp]]

## Integração Socket.IO

O Socket.IO é composto por duas partes:

- Um servidor que se integra com (ou monta em) o servidor HTTP Node.JS [socket.io](https://github.com/socketio/socket.io)
- Uma biblioteca cliente que carrega no lado do navegador [socket.io-cliente](https://github.com/socketio/socket.io-client)

Durante o desenvolvimento, `socket.io` serve o cliente automaticamente para nós, como weliscll ver, então por enquanto só temos que instalar um módulo:

```node
npm install socket.io
```

Isso irá instalar o módulo e adicionar a dependência para `package.json`. Agora vamos edita `index.js` para adicioná-lo:

```js
const express = require('express');
const app = express();
const http = require('http');
const server = http.createServer(app);
const { Server } = require("socket.io");
const io = new Server(server);

app.get('/', (req, res) => {
  res.sendFile(__dirname + '/index.html');
});

io.on('connection', (socket) => {
  console.log('a user connected');
});

server.listen(3000, () => {
  console.log('listening on *:3000');
});
```

Observe que inicializo uma nova instância de `socket.io` passando o `server` (o servidor HTTP) objeto. Então eu escuto no `connection` evento para soquetes de entrada e registrá-lo para o console.

Agora em index.html adicione o seguinte trecho antes do `</body>` (etiqueta do corpo final):

```html
<script src="/socket.io/socket.io.js"></script>
<script>
  var socket = io();
</script>
```

Isso é tudo o que é preciso para carregar o `socket.io-client`, que expõe um `io` global (e o ponto final `GET /socket.io/socket.io.js`), e então conecte.

Se você gostaria de usar a versão local do arquivo JS do lado do cliente, você pode encontrá-lo em `node_modules/socket.io/client-dist/socket.io.js`.

> [!tip]
> Você também pode usar um CDN em vez dos arquivos locais (por exemplo. `<script src="https://cdn.socket.io/4.7.2/socket.io.min.js"></script>`).

Observe que não estão especificando qualquer URL quando eu chamo `io()`, uma vez que o padrão é tentar se conectar ao host que serve a página.

> [!NOTE]
> Se você está por trás de um proxy reverso, como apache ou nginx, por favor dê uma olhada [a documentação para isso](https://socket.io/docs/v4/reverse-proxy/).
>
>
>Se você estiver hospedando seu aplicativo em uma pasta que é _não_ a raiz do seu site (por exemplo `https://example.com/chatapp`) então você também precisa especificar o [caminho](https://socket.io/docs/v4/server-options/#path) tanto no servidor quanto no cliente.

Se agora reiniciar o processo (atacando Control+C e executando `node index.js` novamente) e, em seguida, atualizar a página da web você deve ver o console imprimir “a user connected”.

Tente abrir várias guias e você verá várias mensagens.

![[Um console exibindo várias mensagens, indicando que alguns usuários se conectaram.webp]]

Cada soquete também dispara um especial `disconnect` evento:

```js
io.on('connection', (socket) => {
  console.log('a user connected');
  socket.on('disconnect', () => {
    console.log('user disconnected');
  });
});
```

Então, se você atualizar uma guia várias vezes, poderá vê-la em ação.

![[Um console exibindo várias mensagens, indicando que alguns usuários se conectaram e se desconectaram.webp]]
## Emitindo eventos[](https://socket.io/get-started/chat#emitting-events "Link direto para Emitir eventos")

A ideia principal por trás do Socket.IO é que você pode enviar e receber todos os eventos desejados, com os dados desejados. Quaisquer objetos que possam ser codificados como JSON farão, e [dados binários](https://socket.io/blog/introducing-socket-io-1-0/#binary) também é suportado.

Vamos fazer com que, quando o usuário digita uma mensagem, o servidor a receba como um `chat message` evento. O `script` seção em `index.html` agora deve olhar como segue:

```html
<script src="/socket.io/socket.io.js"></script>
<script>
  var socket = io();

  var form = document.getElementById('form');
  var input = document.getElementById('input');

  form.addEventListener('submit', function(e) {
    e.preventDefault();
    if (input.value) {
      socket.emit('chat message', input.value);
      input.value = '';
    }
  });
</script>
```

E em `index.js` imprimimos o `chat message` evento:

```js
io.on('connection', (socket) => {
  socket.on('chat message', (msg) => {
    console.log('message: ' + msg);
  });
});
```

O resultado deve ser como o seguinte vídeo:

![[zboNrGSsai.mp4]]
## Transmissão

O próximo objetivo é emitir o evento do servidor para o resto dos usuários.
Para enviar um evento para todos, o Socket.IO nos dá o `io.emit()` método.

```js
io.emit('some event', { someProperty: 'some value', otherProperty: 'other value' }); // This will emit the event to all connected sockets
```

Se você quiser enviar uma mensagem para todos, exceto para um determinado soquete emissor, temos o `broadcast` bandeira para emitir a partir desse soquete:

```js
io.on('connection', (socket) => {
  socket.broadcast.emit('hi');
});
```

Nesse caso, por uma questão de simplicidade, enviaremos a mensagem para todos, incluindo o remetente.

```js
io.on('connection', (socket) => {
  socket.on('chat message', (msg) => {
    io.emit('chat message', msg);
  });
});
```

E do lado do cliente quando capturamos um `chat message` evento vamos incluí-lo na página. O _total_ o código JavaScript do lado do cliente agora equivale a:

```html
<script src="/socket.io/socket.io.js"></script>
<script>
  var socket = io();

  var messages = document.getElementById('messages');
  var form = document.getElementById('form');
  var input = document.getElementById('input');

  form.addEventListener('submit', function(e) {
    e.preventDefault();
    if (input.value) {
      socket.emit('chat message', input.value);
      input.value = '';
    }
  });

  socket.on('chat message', function(msg) {
    var item = document.createElement('li');
    item.textContent = msg;
    messages.appendChild(item);
    window.scrollTo(0, document.body.scrollHeight);
  });
</script>
```

E isso completa nosso aplicativo de bate-papo, em cerca de 20 linhas de código! É assim que parece:

## Trabalho de casa[](https://socket.io/get-started/chat#homework "Link direto para o dever de casa")

Aqui estão algumas ideias para melhorar a aplicação:

- [ ] Transmita uma mensagem para usuários conectados quando alguém se conecta ou desconecta.
- [ ] Adicionar suporte para apelidos.
- [ ] Não envie a mesma mensagem para o usuário que a enviou. Em vez disso, anexe a mensagem diretamente assim que pressionar Enter.
- [ ] Adicionar a funcionalidade “{user} esta digitando”.
- [ ] Mostrar quem esta online.
- [ ] Adicionar mensagens privadas.
- [ ] Compartilhe suas melhorias!

## Obtendo este exemplo[](https://socket.io/get-started/chat#getting-this-example "Link direto para Obter este exemplo")

Você pode encontrá-lo no GitHub [aqui](https://github.com/socketio/chat-example).

```
git clone https://github.com/socketio/chat-example.git
```

[[https://socket.io/get-started/chat|Link do documento]]
