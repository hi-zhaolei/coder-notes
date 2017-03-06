# Linux命令

## ps

**ps**是**Process Status**的缩写，用来列出系统中当前运行的那些进程。**ps**命令列出的是当前进程的快照，就是执行**ps**命令这个时刻的进程，可以使用**top**命令获取动态的进程信息。

### 常用命令参考

* 显示所有进程  ps -A
* 显示指定用户进程  ps -u [username]
* 显示所有进程，包括命令行提示符信息 ps -ef
* 显示所有正在内存中进程，展示进程占用系统资源信息  ps -aux
* ps与grep组合使用，查找cmd匹配指定内容的进程  ps -aux|grep php
* 列出命令行相关的进程  ps -l
* 树状结构展示所有的进程 ps -axjf
* 显示进程信息，并记录到指定文件中(指定文件名不存在则默认创建该文件)  ps -aux > log.txt

## netstat

netstat 命令用于显示各种网络相关信息，如网络连接，路由表，接口状态(Interface Statistics),masquerade连接，多播成员(Multicast Memberships)等

命令的输出有两部分：

* 第一部分是Active Internet connections，称为有源TCP连接，其中"Recv-Q"和"Send-Q"指的是接收队列和发送队列，这些数字一般都应该是0，如果不是则表示软件包正在队列中堆积。

* 第二部分是Active UNIX domain sockets，称为有源Unix域套接口(和网络套接字一样，但是只能用于本机通信，性能比网络套接字高一倍)。Proto显示连接使用的协议,RefCnt表示连接到本套接口上的进程号,Types显示套接口的类型,State显示套接口当前的状态,Path表示连接到套接口的其它进程使用的路径名。

### 命令参数

netstat命令默认是不显示LISTEN状态的网络连接和LISTEING状态的UNIX域连接，只有使用带-a或者-l参数的命令才能显示出来。

* -a (all)显示所有状态的连接
* -t (tcp)仅显示tcp相关连接
* -u (udp)仅显示udp相关连接
* -n 拒绝显示别名，能显示数字的全部转化成数字
* -l 仅列出有在监听状态的连接
* -p 显示建立相关链接的程序名
* -r 显示路由信息，路由表
* -e 显示扩展信息，例如uid等
* -s 按各个协议进行统计
* -c 每隔一个固定时间，执行该netstat命令

### 常用命令

* 列出所有连接（包括监听和未监听状态）
    * 命令：netstat -a；列出所有状态下的连接
    * 命令：netstat -at；列出所有状态下的tcp连接
    * 命令：netstat -au；列出所有状态下的udp连接
    * 命令：netstat -ax；列出所有状态下的UNINX域连接
* 列出处于LISTEN状态的连接
    * 命令：netstat -l；列出所有处于LISTEN状态的连接
    * 命令：netstat -lt；列出所有处于LISTEN状态的tcp连接
    * 命令：netstat -lu；列出所有处于LISTEN状态的udp连接
    * 命令：netstat-lx；列出所有处于LISTENING状态的UNIX域连接
* 统计通信协议连接信息
    * 命令：netstat -s；统计所有连接的通信协议连接信息
    * 命令：netstat -st；统计基于tcp连接的通信协议连接信息
    * 命令：netstat -su；统计基于udp连接的通信协议连接信息
* 输出中显示进程ID和进程名信息（可搭配其他参数使用）
    * 命令：netstat -p；列出除LISTEN和LISTENING状态下的连接，包含连接所属进程的进程ID和进程名
    * 命令：netstat -tp；列出除LISTEN和LISTENING状态下的tcp连接，包含连接所属进程的进程ID和进程名
    * 命令：netstat -up；列出除LISTEN和LISTENING状态下的udp连接，包含连接所属进程的进程ID和进程名
* 动态输出连接信息
    * 命令：netstat -c；每间隔一秒输出当前连接信息
* 列出特定的连接
    * 命令：netstat -ap|grep postgres