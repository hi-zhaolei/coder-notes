# HTTP

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
