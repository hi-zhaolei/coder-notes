# HTTPS

## 什么是HTTPS

**HTTPS**是安全的**HTTP**，是 HTTP 协议和 SSL/TLS 协议的组合。

HTTP 协议中的内容都是明文传输的, 而 HTTPS 的目的是将这些内容加密, 确保信息传输安全.

信息的明文传递带来了三大风险

* 窃听风险（eavesdropping）：第三方可以获知通信内容.

* 篡改风险（tampering）：第三方可以修改通信内容.

* 冒充风险（pretending）：第三方可以冒充他人身份参与通信.

信息加密传递的解决方法

* 客户端和服务器直接的通信都是加密传输, 即使第三方拿到数据也无法窃取真正信息

* 第三方虽然看不懂数据, 但可以篡改, 因此客户端和服务器必须具有校验机制, 一旦被篡改, 通信双方会立刻发现

* 配备身份证书, 避免中间人攻击, 防止服务器身份被冒充

## 信息加密方法

**HTTPS**最后一个字母**S**指的是**SSL/TLS**协议, 它位于**HTTP**协议与**TCP/IP**协议中间.

SSL(Secure Sockets Layer)中文叫做“安全套接层”。它是在上世纪90年代中期，由网景公司设计的。因为原先互联网上使用的 HTTP 协议是明文的，存在很多缺点——比如传输内容会被偷窥和篡改。
发明 SSL 协议，就是为了解决这些问题。到了1999年，SSL 因为应用广泛，已经成为互联网上的事实标准。IETF 就在那年把 SSL 标准化。
标准化之后的名称改为 TLS（是“Transport Layer Security”的缩写），中文叫做“传输层安全协议”。这两者可以视作同一个东西的不同阶段。

>1994年, NetScape公司设计了SSL协议（Secure Sockets Layer）的1.0版, 但是未发布.
>1995年, NetScape公司发布SSL 2.0版, 很快发现有严重漏洞.
>1996年, SSL 3.0版问世, 得到大规模应用.
>1999年, 互联网标准化组织ISOC接替NetScape公司, 发布了SSL的升级版TLS 1.0版.
>2006年和2008年, TLS进行了两次升级, 分别为TLS 1.1版和TLS 1.2版. 最新的变动是2011年TLS 1.2的修订版.

目前, 应用最广泛的是TLS 1.0, 接下来是SSL 3.0. 但是, 主流浏览器都已经实现了TLS 1.2的支持.
TLS 1.0通常被标示为SSL 3.1, TLS 1.1为SSL 3.2, TLS 1.2为SSL 3.3.

SSL/TLS协议的基本思路是采用[公钥加密法](http://en.wikipedia.org/wiki/Public-key_cryptography)。
基本过程是这样的：

1.客户端向服务器端索要并验证公钥。

2.双方协商生成"对话密钥"。

3.双方改成对称加密，采用"对话密钥"进行加密通信。

## 详细协议握手过程

1 客户端发送https请求, 给出协议版本号(eg: TLS 1.2)，
一个客户端生成的**1.随机数(Client random)**, **客户端支持的加密算法**以及**客户端支持的压缩方法**, 请求服务器建立连接(ClientHello)

2 服务端确认双方的加密通信协议版本(eg: TLS 1.2), 信息加密方法(eg: RSA), 数字证书(通过**证书私钥**加密的**服务端公钥**)以及一个服务器**2.随机数(Server random)**回复给客户端(SeverHello)

3 客户端拿到回复, 首先确认证书合法性(颁发证书的机构是否合法，证书中包含的网站地址是否与正在访问的地址一致等，如果证书受信任，则浏览器栏里面会显示一个小锁头), 
然后通过本地**证书公钥**解密数字证书获取**服务端公钥**, 再将生成一个新的**3.随机数(Premaster secret)**, **编码改变通知(改为对称加密)**以及**客户端握手结束通知(内容hash, 供服务端校验)**通过**服务端公钥**加密后, 发送给服务端, 
最后根据约定的加密方法, 使用1.2.3三个随机数生成**"对话密钥"(session key)**

4 服务端拿到数据通过**服务端私钥**解密, 拿到客户端的**随机数(Premaster secret)**, 根据约定的加密方法, 使用1.2.3三个随机数生成**对话密钥(session key)**, 
返回客户端**编码改变通知(改为对称加密)**以及**客户端握手结束通知(内容hash, 供客户端校验)**, 确认可以开始数据传输, 使用普通**HTTP**协议传输, **对话密钥(session key)**加密内容

![TLS handshake](../../img/TLS.png)

握手阶段有三点需要注意:

1.生成对话密钥一共需要三个随机数。

2.握手之后的对话使用"对话密钥"加密（对称加密），服务器的公钥和私钥只用于加密和解密"对话密钥"（非对称加密），无其他作用，所以整个对话过程中（握手阶段和其后的对话），服务器的公钥和私钥只需要用到一次。

3.服务器公钥放在服务器的数字证书之中。

整个握手阶段都不加密（也没法加密），都是明文的。因此，如果有人窃听通信，他可以知道双方选择的加密方法，以及三个随机数中的两个。
整个通话的安全，只取决于第三个随机数（Premaster secret）能不能被破解。

### 非对称加密

非对称加密算法：RSA，DSA/DSS

因为计算两个质数的乘积很容易, 但反过来分解成两个质数的乘积就很难, 要经过极为复杂的运算.
非对称加密有两个秘钥, 一个是公钥, 一个是私钥. 公钥加密的内容只有私钥可以解密, 私钥加密的内容只有公钥可以解密.
一般我们把服务器自己留着且不对外公布的密钥称为私钥, 所有人都可以获取的称为公钥.

* 服务器下发的内容不可能被伪造, 因为私钥为服务器独有, 强行加密的后果是客户端用公钥无法解开.

* 任何人用公钥加密的内容都是绝对安全的, 因为私钥为服务器独有, 也只有服务器可以看到被加密的原文.

#### 公钥传输

**服务端公钥**传输给客户端时需要保证不被篡改, 解决办法是将公钥放在[数字证书](http://en.wikipedia.org/wiki/Digital_certificate)中。只要校验证书是合法的, 公钥就是可信的。

但是这并不能防止证书被篡改, 为了确保原始证书的正确性, 我们可以在传递证书的同时传递证书的哈希值. 由于第三者无法解析数据, 只能篡改, 那么修改后的数据在解密后, 就不可能通过哈希校验

#### 数字证书

每一个使用**HTTPS**的服务器都必须去专门的证书机构注册一个证书, 证书中存储了用权威机构私钥加密的公钥. 这样客户端用本地证书中权威机构的公钥解密就可以了.

证书的公钥并不需要传输, 它会直接内置在各大操作系统(或者浏览器)的出厂设置里.

### 对称加密

对称加密算法：AES，RC4，3DES

虽然非对称加密安全, 但是增加了服务器的额外开销, 影响信息传输速度.

对称加密是指对原始数据的可逆变换, 服务端和客户端使用相同的密钥, 既可以加密也可以解密

所以每一次对话（session）, 客户端和服务器端都生成一个"对话密钥"（session key）, 用它来加密信息.
由于"对话密钥"是对称加密, 所以运算速度非常快, 而服务器公钥只用于加密"对话密钥"本身, 这样就减少了加密运算的消耗时间.

### HASH算法

HASH算法：MD5，SHA1，SHA256

## session的恢复

握手阶段用来建立SSL连接. 如果出于某种原因, 对话中断, 就需要重新握手.

这时有两种方法可以恢复原来的session：一种叫做session ID, 另一种叫做session ticket.

### session ID

session ID的思想很简单, 就是每一次对话都有一个编号（session ID）. 如果对话中断, 下次重连的时候, 只要客户端给出这个编号, 且服务器有这个编号的记录, 双方就可以重新使用已有的"对话密钥", 而不必重新生成一把.

![session ID](../../img/session_ID.png)

session ID是目前所有浏览器都支持的方法, 但是它的缺点在于session ID往往只保留在一台服务器上.

### session ticket

客户端不再发送session ID, 而是发送一个服务器在上一次对话中发送过来的session ticket. 这个session ticket是加密的, 只有服务器才能解密, 其中包括本次对话的主要信息, 比如对话密钥和加密方法. 当服务器收到session ticket以后, 解密后就不必重新生成对话密钥了.

![session ticket](../../img/session_ticket.png)

--EOF--
