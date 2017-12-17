# 跨域

跨域，是指浏览器不能执行其他网站的脚本。它是由浏览器的同源策略造成的，是浏览器对JavaScript实施的安全限制。

同源策略限制了一下行为：

*Cookie、LocalStorage 和 IndexDB 无法读取

*DOM 和 JS 对象无法获取

*Ajax请求发送不出去

## 跨域场景

```auto
http://www.a.cn/index.html 调用   http://www.a.cn/index.php  非跨域

http://www.a.cn/index.html 调用   http://www.b.cn/index.php  跨域,主域不同

http://abc.a.cn/index.html 调用   http://def.b.cn/index.php  跨域,子域名不同

http://www.a.cn:8080/index.html 调用   http://www.a.cn/index.php  跨域,端口不同

https://www.a.cn/index.html 调用   http://www.a.cn/index.php  跨域,协议不同
```

## 方法

### jsonp

jsonp跨域其实也是JavaScript设计模式中的一种代理模式。

在html页面中通过相应的标签从不同域名下加载静态资源文件是被浏览器允许的。

一般，我们可以动态的创建script标签，再去请求一个网址来实现跨域通信

```js
//原生的实现方式
let script = document.createElement('script');
script.src = 'http://www.any.com/?callback=callback';
document.body.appendChild(script);

function callback(res) {
  console.log(res);
}
```

虽然jsonp方式非常好用，但是它只能够实现get请求

### document.domain iframe跨域

这种跨域的方式要求主域名相同，也就是当前域名和请求域名必须相同。

利用 iframe 加载其他域下的文件, 原文件和 iframe 文件都将 document.domain 设置相同，在 iframe 文件中请求同主域接口

### window.name iframe跨域

window.name属性可设置或者返回存放窗口名称的一个字符串。
它的值在不同页面或者不同域下加载后依旧存在，没有修改就不会发生变化，并且可以存储2MB以下的内容。

假设index页面请求远端服务器上的数据，我们在该页面下创建iframe标签，
该iframe的src指向服务器文件地址，服务器文件里将数据设置为window.name的值，
由于 iframe 如果和当前文件不同域，是无法操作和读取 iframe 里数据的，
所以在iframe载入过程中，迅速重置iframe.src的指向，使之与index.html同源，那么index页面就能去获取它的name值了



```js
iframe = document.createElement('iframe'),
iframe.src = 'http://localhost:8080/data.php';
document.body.appendChild(iframe);
iframe.onload = function() {
  iframe.src = 'http://localhost:81/cross-domain/proxy.html';
  console.log(iframe.contentWindow.name)
};
```

利用全局对象属性的方法，缺点和jsonp也是一样的，就是只能够实现get请求

### location.hash iframe 跨域

和 window.name 一样是动态插入一个iframe然后设置其src为服务端地址，而服务端同样输出一端js代码，也同时通过与子窗口之间的通信来完成数据的传输。
不同的是，window.name 是吧数据写到 window.name 上，这里是写到 location.hash 上。


利用全局对象属性的方法，缺点和jsonp也是一样的，就是只能够实现get请求。

### postMessage

这是由H5提出来的一个炫酷的API，IE8+，chrome,ff都已经支持实现了这个功能。

发送信息的postMessage方法是向外界窗口发送信息

```js
Window.postMessage(message,targetOrigin);
```

接受信息的message事件

```js
if(typeof window.addEventListener != 'undefined'){
    window.addEventListener('message',onmessage,false);
}else if(typeof window.attachEvent != 'undefined'){
    window.attachEvent('onmessage', onmessage);
}
```

### CORS

CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）。

它允许浏览器向跨源服务器，发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制。

CORS需要浏览器和服务器同时支持。目前，所有浏览器都支持该功能，IE浏览器不能低于IE10。IE8+：IE8/9需要使用XDomainRequest对象来支持CORS。

整个CORS通信过程，都是浏览器自动完成，不需要用户参与。CORS通信与同源的AJAX通信没有差别，代码完全一样。
浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求。

#### 请求

请求分两种

*请求方式为HEAD、POST 或者 GET

*http头信息不超出一下字段：Accept、Accept-Language 、 Content-Language、 Last-Event-ID、 Content-Type(限于三个值：application/x-www-form-urlencoded、multipart/form-data、text/plain)

#### 简单请求

请求方式为HEAD、POST 或者 GET

对于简单请求，浏览器直接发出CORS请求。在头信息之中，增加一个Origin字段。
Origin字段用来说明，本次请求来自哪个源（协议 + 域名 + 端口）。服务器会根据这个值，决定是否同意这次请求。

```auto
GET /cors HTTP/1.1
Origin: http://api.bob.com
Host: api.alice.com
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0
...
```

如果Origin指定的源，不在许可范围内，服务器会返回一个正常的HTTP回应。
当浏览器发现回应的头信息没有包含Access-Control-Allow-Origin字段，会抛出一个错误，被XMLHttpRequest的onerror回调函数捕获。

这种错误无法通过状态码识别的。

如果 Origin 指定的域名在许可范围内，服务器响应会多出几个头信息字段。

```auto
Access-Control-Allow-Origin: http://api.bob.com
Access-Control-Allow-Credentials: true
Access-Control-Expose-Headers: FooBar
Content-Type: text/html; charset=utf-8
```

*Access-Control-Allow-Origin :必须。它的值要么是请求时Origin字段的值，要么是一个\*，表示接受任意域名的请求

*Access-Control-Allow-Credentials: 可选。它的值是一个布尔值，表示是否允许发送Cookie。默认情况下，Cookie不包括在CORS请求之中。设为true，即表示服务器明确许可，Cookie可以包含在请求中，一起发给服务器。这个值也只能设为true，如果服务器不要浏览器发送Cookie，删除该字段即可。

*Access-Control-Expose-Headers:可选。CORS请求时，XMLHttpRequest对象的getResponseHeader()方法只能拿到6个基本字段：Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma。如果想拿到其他字段，就必须在Access-Control-Expose-Headers里面指定。

CORS把Cookie发到服务器，既要服务器同意，指定Access-Control-Allow-Credentials字段，还需要开发者必须在AJAX请求中打开withCredentials属性。

```js
// 前端设置是否带cookie
xhr.withCredentials = true;
```

如果要发送Cookie，Access-Control-Allow-Origin字段就不能设为星号，必须指定明确的、与请求网页一致的域名。
同时，Cookie依然遵循同源政策，只有用服务器域名设置的Cookie才会上传。

#### 复杂请求

复杂请求是对服务器有特殊要求，比如请求方法是PUT或DELETE，或者Content-Type字段的类型是application/json。

复杂请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为"预检"请求（preflight）。

浏览器先询问服务器，当前网页所在的域名是否在服务器的白名单之中，以及可以使用的HTTP动词和头信息字段。
只有得到服务器的肯定答复，浏览器才会发出正式的XMLHttpRequest请求，否则就报错。

eg.

```js
var url = 'http://www.baidu.com/';
var xhr = new XMLHttpRequest();
xhr.open('PUT', url, true);
xhr.setRequestHeader('X-Custom-Header', '123');
xhr.send();
```

当前浏览器发现这是不是简单请求后，就自动发出一个"预检"，要求服务器确认。

这是这个"预检"请求的HTTP头信息

```auto
OPTIONS /cors HTTP/1.1
Origin: http://xxxxxx
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: X-Custom-Header
Host: xxxxxx
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...
```

"预检"请求用的请求方法是OPTIONS，表示是用来询问的。头信息里面，关键字段是Origin，表示请求来自哪个源。

除了Origin字段，"预检"请求的头信息包括两个特殊字段。

*Access-Control-Request-Method：该字段是必须的，用来列出浏览器的CORS请求会用到哪些HTTP方法，上例是PUT。

*Access-Control-Request-Headers：该字段是一个逗号分隔的字符串，指定浏览器CORS请求会额外发送的头信息字段，上例是X-Custom-Header

服务器收到"预检"请求以后，检查Origin、Access-Control-Request-Method和Access-Control-Request-Headers字段，确认允许跨源请求并做出回应

```auto
HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 01:15:39 GMT
Server: Apache/2.0.61 (Unix)
Access-Control-Allow-Origin: http://xxxxxx
Access-Control-Allow-Methods: GET, POST, PUT
Access-Control-Allow-Headers: X-Custom-Header
Content-Type: text/html; charset=utf-8
Content-Encoding: gzip
Content-Length: 0
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
Content-Type: text/plain
```

在HTTP响应头中，关键的是Access-Control-Allow-Origin字段，表示可以请求数据。该字段也可以设为星号，表示同意任意跨源请求。

如果浏览器否定了"预检"请求，会返回一个正常的HTTP回应，但是没有任何CORS相关的头信息字段。

其他想相关字段

*Access-Control-Allow-Methods：该字段必需，它的值是逗号分隔的一个字符串，表明服务器支持的所有跨域请求的方法。注意，返回的是所有支持的方法，而不单是浏览器请求的那个方法。这是为了避免多次"预检"请求。

*Access-Control-Allow-Headers：如果浏览器请求包括Access-Control-Request-Headers字段，则Access-Control-Allow-Headers字段是必需的。它也是一个逗号分隔的字符串，表明服务器支持的所有头信息字段，不限于浏览器在"预检"中请求的字段。

*Access-Control-Allow-Credentials： 该字段与简单请求时的含义相同。

*Access-Control-Max-Age： 该字段可选，用来指定本次预检请求的有效期，单位为秒。上面结果中，有效期是20天（1728000秒），即允许缓存该条回应1728000秒（即20天），在此期间，不用发出另一条预检请求。

一旦服务器通过了"预检"，以后每次浏览器正常的CORS请求，就都跟简单请求一样，会有一个Origin头信息字段。服务器的回应，也都会有一个Access-Control-Allow-Origin头信息字段。

### WebSocket协议

WebSocket protocol是HTML5一种新的协议。它实现了浏览器与服务器全双工通信，同时允许跨域通讯，是server push技术的一种很好的实现。

原生WebSocket API使用起来不太方便，我们使用Socket.io，它很好地封装了webSocket接口，提供了更友好的接口，也对不支持webSocket的浏览器进行向下兼容。

```html
<!-- a.html -->
<div>user input：<input type="text"></div>
<script src="./socket.io.js"></script>
<script>
var socket = io('http://www.b.com:8080');

// connect success
socket.on('connect', function() {
    // listen server message
    socket.on('message', function(msg) {
        console.log('data from server: ' + msg); 
    });
    // close service
    socket.on('disconnect', function() { 
        console.log('Socket closed.'); 
    });
});

document.getElementsByTagName('input')[0].onblur = function() {
    socket.send(this.value);
};
</script>
```

```js
var http = require('http');
var socket = require('socket.io');
// http service start
var server = http.createServer(function(req, res) {
  res.writeHead(200, {'Content-type': 'text/html'});
  res.end();
});
server.listen('8080');
console.log('Server is running at port 8080...');
// listen socket
socket.listen(server).on('connection', function(client) {
  // receive message
  client.on('message', function(msg) {
    client.send('hello：' + msg);
    console.log('data from client: ' + msg);
  });
  // close socket
  client.on('disconnect', function() {
    console.log('Client socket has closed.');
  });
});
```

### node代理跨域

node中间件实现跨域代理，其实就是通过启一个代理服务器，实现数据的转发，
也可以通过设置 cookieDomainRewrite 参数修改响应头中 cookie中 域名，实现当前域的 cookie 写入，方便接口登录认证。

```js
var xhr = new XMLHttpRequest();

// 前端开关：浏览器是否读写cookie
xhr.withCredentials = true;

// 访问http-proxy-middleware代理服务器
xhr.open('get', 'http://www.a.com:8080/xxx', true);
xhr.send();
```

```js
var express = require('express');
var proxy = require('http-proxy-middleware');
var app = express();

app.use('/', proxy({
    // 代理跨域目标接口
    target: 'http://www.b.com:8080',
    changeOrigin: true,

    // 修改响应头信息，实现跨域并允许带cookie
    onProxyRes: function(proxyRes, req, res) {
        res.header('Access-Control-Allow-Origin', 'http://www.a.com');
        res.header('Access-Control-Allow-Credentials', 'true');
    },

    // 修改响应信息中的cookie域名
    cookieDomainRewrite: 'www.a.com'
}));

app.listen(8080);
console.log('Proxy server is listen at port 8080...');
```