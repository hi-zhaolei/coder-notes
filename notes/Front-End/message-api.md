# HTML5 postMessage 和 onmessage API

HTML5 不但强化了 Web 系统或网页的表现性能，而且还增加了对本地数据库等 Web 应用功能的支持。其中，最重要的一个便是对多线程的支持。

## Web Worker

HTML5 中提出了工作线程（Web Workers）的概念，并且规范出 Web Workers 的三大主要特征：能够长时间运行（响应），理想的启动性能以及理想的内存消耗。

Web Workers 允许开发人员编写能够长时间运行而不被用户所中断的后台程序，去执行事务或者逻辑，并同时保证页面对用户的及时响应。

Web Workers 为 Web 前端网页上的脚本提供了一种能在后台进程中运行的方法。

Web Workers 可以通过 postMessage 向任务池发送任务请求，执行完之后再通过 postMessage 返回消息给创建者指定的事件处理程序 ( onmessage 捕获事件 )。

Web Workers 进程能够在不影响用户界面的情况下处理任务，并且，它还可以使用 XMLHttpRequest 来处理 I/O，但后台进程（包括 Web Workers 进程）不能对 DOM 进行操作。

```js
function createWorker(){
  const worker = new Worker('worker.js');
  worker.onmessage= function (event) {
    //event 参数中有 data 属性，就是子线程中返回的结果数据
    console.log(event);
  };
}
```

```js
// worker.js
onmessage = function (event) {
  //event 参数中有 data 属性，主线程中发送的数据
  const data = event.data;
  postMessage(data);
}
```

## Cross-document messaging

由于同源策略的限制，JavaScript 跨域的问题一直是棘手的问题。

HTML5 提供了在网页文档之间互相接收与发送信息的功能。使用这个功能，只要获取到网页所在窗口对象的实例，不仅同源（域 + 端口号）的 Web 网页之间可以互相通信，甚至可以实现跨域通信。

要想接收从其他窗口发送来的信息，必须对窗口对象的 onmessage 事件进行监听，其它窗口可以通过 postMessage 方法来传递数据。

该方法使用两个参数：第一个参数为所发送的消息文本，但也可以是任何 JavaScript 对象（通过 JSON 转换对象为文本），第二个参数为接收消息的对象窗口的 URL 地址，可以在 URL 地址字符串中使用通配符'*'指定全部地。

## WebSockets

[WebSockets](../Network/web-socket.md)