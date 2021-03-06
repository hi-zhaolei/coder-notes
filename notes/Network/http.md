# HTTP

HTTP 是一个网络协议，是专门用来帮你传输 Web 内容滴。

网站都是通过 HTTP 协议来传输 Web 页面、以及 Web 页面上包含的各种图片、CSS 样式、JS 脚本等。

## TCP链接

HTTP 对 TCP 连接的使用，分为两种方式：俗称“短连接”和“长连接”（“长连接”又称“持久连接”，洋文叫做“Keep-Alive”或“Persistent Connection”）

**短链接**是在请求到HTML数据后直接关闭 TCP 链接，在需要请求页面其他资源时分别发起 TCP 连接，把这些文件获取到本地。

**长链接**是在请求到HTML数据后直接不会 TCP 链接，保持 TCP 链接 Keep-Alive，在需要请求其他资源时使用保持的 TCP 链接，无需再次建立服务器链接。

## 缓存

### 通用首部字段

#### Cache-Control

控制缓存行为

针对上述的“Expires时间是相对服务器而言，无法保证和客户端时间统一”的问题，http1.1新增了 Cache-Control 来定义缓存过期时间。

若报文中同时出现了 Expires 和 Cache-Control，则以 Cache-Control 为准。也就是说优先级从高到低分别是 **Pragma -> Cache-Control -> Expires**

Cache-Control也是一个通用首部字段，这意味着它能分别在请求报文和响应报文中使用。在RFC中规范了 Cache-Control 的格式为：`"Cache-Control" ":" cache-directive`

作为请求首部时，cache-directive 的可选值有：

* no-cache 告知服务器不直接使用缓存，要求向原服务器发起请求
* no-store 所有内容都不会保存到缓存或Internet临时文件中
* max-age=delta-seconds    告知服务器客户端希望接收一个存在时间不大于**delta-seconds**秒的资源
* max-stale[=delta-seconds]    愿意接受一个超过缓存时间的资源，若有定义**de**

#### Pragma

http1.0，值为**no-cache**时禁用缓存

### 请求首部字段

#### If-Match

比较ETag是否一致

#### If-None-Match

比较ETag是否不一致

#### If-Modified-Since

比较资源最后更新的事件是否一致

#### If-Unmodified-Since

比较资源最后更新的时间是否不一致

### 响应首部字段

#### ETag

资源匹配信息

### 实体首部字段

#### Expires

http1.0，实体主体过期时间

#### Last-Modified

资源的最后一次修改时间
