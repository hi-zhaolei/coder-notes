# HTTPS

## 什么是HTTPS

**HTTPS**是安全的**HTTP**

HTTP 协议中的内容都是明文传输，HTTPS 的目的是将这些内容加密，确保信息传输安全。最后一个字母 S 指的是 SSL/TLS 协议，它位于 HTTP 协议与 TCP/IP 协议中间。

## 信息传输安全

* 客户端和服务器直接的通信只有自己能看懂，即使第三方拿到数据也看不懂这些信息的真实含义

* 第三方虽然看不懂数据，但可以**XJB**改，因此客户端和服务器必须有能力判断数据是否被修改过

* 客户端必须避免中间人攻击，即除了真正的服务器，任何第三方都无法冒充服务器

## 加密信息

对称加密可以理解为对原始数据的可逆变换。比如 Hello 可以变换成 Ifmmp，规则就是每个字母变成它在字母表上的后一个字母，这里的秘钥就是 1


### 对称加密

### 非对称加密