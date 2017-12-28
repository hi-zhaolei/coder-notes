# WebSocket

在 Web 应用中，HTTP 协议决定了客户端和服务端连接是短连接，即客户端 Request，服务端 Response，连接断开。
要想实现客户端和服务端实时通信，只能通过客户端轮询来实现。
服务端推送数据也并不是字面上意思上的直接推，其实还是客户端自己取。

WebSockets 是 HTML5 规范新引入的功能，用于解决浏览器与后台服务器双向通讯的问题，使用 WebSockets 技术，后台可以随时向前端推送消息，以保证前后台状态统一。

## 例子

```js
// client code
function createWebSocket() {
  var location ="ws://localhost:8000/";
  // 创建 WebSockets 并传入 WebSockets server 地址
  ws =new WebSocket(location);
  ws.onmessage=onmessage;
  //WebSockets 还提供了 onopen 以及 onclose 事件
  ws.onopen=onopen;
  ws.onclose=onclose;

  function onmessage (event) {
    // event data属性，是服务端发送过来的数据
    console.log(event.data);
  }
  function onopen() {
    // 
    wx.send('Client:Open WebSockets');
  }
  function onclose() {
    //
    wx.send('Client:Close WebSockets');
    ws = null;
  }
}

// server code
// 监听客户端的连接请求
wsServer.on('connect', function(connection) {
  // 监听客户端发送数据的请求
  connection.on('message', function(message) {
      // 区别客户端发过来的message.type是文本还是二进制类型
   });
  connection.on('close', function(reasonCode, description) {});
});
```

## Server-Sent Events

HTML5 Server-Sent 事件模型允许您从服务器 push 实时数据到浏览器。

在现存的 Ajax 模式中，web 页面会使用轮训的方式不断请求服务器传输新数据，而在服务端发送模式下，是由服务端 push 推送更新，客户端只负责接受数据。
一旦您在页面中初始化了 Server-Sent 事件，服务端脚本将持续地发送更新。

Server-Sent Events 和 WebSockets 有相同之处，WebSockets 实现了服务器端以及客户端的双向通信功能，而 Server-Sent Events 则仅是指服务器端到客户端的单向通信

Server-Sent Events 同样需要服务器端的实现，关于服务器端向客户端写数据的格式，可以参考 W3C 关于[Server-Sent Events 的规范文档](http://www.w3.org/TR/eventsource/)。
由于是服务器端到客户端的单向通信，所以在 Server-Sent Events 中没有 postMessage 方法。

```js
// client code
if (!!window.EventSource) {
  // 创建 EventSource 实例，传入 server 地址
  const source = new EventSource('server url');
} else {
  console.warn("Your browser doesn't support server-sent event yet!");
}
// 监听 message 事件，等待接收服务器端发送过来的数据
source.addEventListener('message', function(event) {
  //event的 data 属性是服务器发送过来的数据
  console.log(event.data);
}, false);
//EventSource 还提供了 onopen 以及 onerror 事件
```