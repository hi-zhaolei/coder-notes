# Nnginx

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
