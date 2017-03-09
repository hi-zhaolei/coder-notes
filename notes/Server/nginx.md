# Nnginx

## 配置文件

### 定义Nginx运行的用户和用户组

user www www;

### worker_processes

nginx进程数，根据硬件调整
worker_processes 8;

### error_log

全局错误日志定义类型
error_log /var/log/nginx/error.log [ debug | info | notice | warn | error | crit ]

### pid

进程文件存放路径
pid /var/run/nginx.pid

### worker_rlimit_nofile

worker_rlimit_nofile 204800;
这个指令是指当一个nginx进程打开的最多文件描述符数目，理论值应该是最多打开文件数（ulimit -n）与nginx进程数相除，但是nginx分配请求并不是那么均匀，所以最好与ulimit -n 的值保持一致

### events

设定工作模式

#### use

设定事件模型
use [ kqueue | rtsig | epoll | /dev/poll | select | poll ]
epoll模型是Linux 2.6以上版本内核中的高性能网络I/O模型，如果跑在FreeBSD上面，就用kqueue模型。

#### worker_connections

单个进程最大连接数（最大连接数=连接数*进程数）
worker_connections 65535;

### http

服务器配置

#### include

文件扩展名与文件类型映射表
include mime.types

#### default_type

默认文件类型
default_type application/octet-stream

#### charset

默认编码

#### server_names_hash_bucket_size

服务器名字的hash表大小

#### client_header_buffer_size

上传文件大小限制

#### large_client_header_buffers

设定请求缓 FIXME!

#### client_max_body_size

FIXME!

#### sendfile

开启高效文件传输模式，**sendfile**指令指定**nginx**是否调用**sendfile**函数来输出文件，对于普通应用设为 **on**，如果用来进行下载等应用磁盘IO重负载应用，可设置为**off**，以平衡磁盘与网络I/O处理速度，降低系统的负载。注意：如果图片显示不正常把这个改成**off**。

#### autoindex

开启目录列表访问，合适下载服务器，默认**off**

#### tcp_nopush

防止网络阻塞

#### tcp_nodelay

FIXME!

#### keepalive_timeout

长连接超时时间，单位是秒880129

####  FastCGI

* fastcgi_connect_timeout 300;
* fastcgi_send_timeout 300;
* fastcgi_read_timeout 300;
* fastcgi_buffer_size 64k;
* fastcgi_buffers 4 64k;
* fastcgi_busy_buffers_size 128k;
* fastcgi_temp_file_write_size 128k;

#### gzip

* gzip on; 开启gzip压缩输出
* gzip_min_length 1k; 最小压缩文件大小
* gzip_buffers 4 16k; 压缩缓冲区
* gzip_http_version 1.0; 压缩版本（默认1.1，前端如果是squid2.5请使用1.0）
* gzip_comp_level 2; 压缩等级
* gzip_types text/plain application/x-javascript text/css application/xml;压缩类型，默认就已经包含text/html，所以下面就不用再写了，写上去也不会有问题，但是会有一个warn。
* gzip_vary on; #开启限制IP连接数的时候需要使用

#### upstream

upstream的负载均衡

##### server

配置负载均衡服务器
server 192.168.80.121:80 weight=3;
weight是权重，可以根据机器配置定义权重。weigth参数表示权值，权值越高被分配到的几率越大。

#### server

虚拟主机配置

##### listen

监听端口

##### server_name

域名配置，可以有多个，空格隔开

##### index

主页配置，可以有多个，依此寻找，空格隔开

##### root

项目根目录

##### access_log

定义本虚拟主机的访问日志

##### location

基本语法：location [=|~|~*|^~|!~|!~\*|/] /uri/ { … }

* = 严格匹配。如果这个查询匹配，那么将停止搜索并立即处理此请求
* ~ 为区分大小写匹配(可正则)
* ~* 为不区分大小写匹配(可正则)
* ^~ 如果把这个前缀用于一个常规字符串,那么告诉nginx 如果路径匹配那么不测试正则表达式
* !~和!~*分别为区分大小写不匹配及不区分大小写不匹配(可正则)
* / 通用匹配，任何请求都会匹配到

匹配规则：首先匹配 =，其次匹配^~,其次是按文件中顺序的正则匹配，最后是交给 / 通用匹配。当有匹配成功时候，停止匹配，按当前匹配规则处理请求

## Nginx能做什么

### 反向代理

反向代理（Reverse Proxy）是指以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。简单来说就是真实的服务器不能直接被外部网络访问，所以需要一台代理服务器，而代理服务器能被外部网络访问的同时又跟真实服务器在同一个网络环境，当然也可能是同一台服务器，端口不同而已。

```
server {
    listen       80;
    server_name  localhost;
    client_max_body_size 1024M;

    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host:$server_port;
    }
}
```
当我们访问localhost的时候，就相当于访问localhost:8080了

### 负责均衡

负载均衡也是**Nginx**常用的一个功能
负载均衡其意思就是分摊到多个操作单元上进行执行，从而共同完成工作任务。
简单而言就是当有2台或以上服务器时，根据规则随机的将请求分发到指定的服务器上处理，负载均衡配置一般都需要同时配置反向代理，通过反向代理跳转到负载均衡。而**Nginx**目前支持自带3种负载均衡策略，还有2种常用的第三方策略。


### HTTP服务器

**Nginx**本身也是一个静态资源的服务器，当只有静态资源的时候，就可以使用**Nginx**来做服务器，同时现在也很流行动静分离，就可以通过**Nginx**来实现

* 静态服务器

```
server {
    listen  80;
    server_name  localhost;
    client_max_body_size 1024M;

    location / {
         root   /Users/leozhao/Documents/;
         index  index.html;
    }
}
```
这样如果访问*http://localhost* 就会默认访问*/Users/leozhao/Documents/*目录下面的*index.html*

* 动态服务器

```
upstream test{
   server localhost:8080;
   server localhost:8081;
}

server {
    listen  80;
    server_name  localhost;

    location / {
        root   /Users/leozhao/Documents/;
        index  index.html;
    }

    # 所有静态请求都由nginx处理，存放目录为html
    location ~ \.(gif|jpg|jpeg|png|bmp|swf|css|js)$ {
        root    /Users/leozhao/Documents/;
    }

    # 所有动态请求都转发给tomcat处理
    location ~ \.(jsp|do)$ {
        proxy_pass  http://test;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /Users/leozhao/Documents/;
    }
}
```
**HTML**以及**图片**和**css**以及**js**放到*/Users/leozhao/Documents/*目录下，而**tomcat**只负责处理jsp和请求

### 正向代理

正向代理，意思是一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。
正向代理只是相对于客户端。当你需要把你的服务器作为代理服务器的时候，可以用**Nginx**来实现正向代理
