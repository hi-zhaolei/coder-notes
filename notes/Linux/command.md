# Linux命令

## 查看系统信息

`uname -a`  电脑以及操作系统的相关信息

`cat /proc/version` 运行的内核版本

`cat /etc/issue`    发行版本信息

## curl

网络请求, 相关的还有 traceroute, dig 等

## find

文件查找，使用格式如下：

$ find <指定目录> <指定条件> <指定动作>

- <指定目录>： 所要搜索的目录及其所有子目录。默认为当前目录。

- <指定条件>： 所要搜索的文件的特征。

- <指定动作>： 对搜索结果进行特定的处理。

### 常用参数

文件名 -name

文件类型 -type

查找最大深度 -maxdepth

时间过滤(create/access/modify) -[cam]time

执行动作 -exec

## locate

locate命令其实是"find -name"的另一种写法，但是要比后者快得多，原因在于它不搜索具体目录，而是搜索一个数据库（/var/lib/locatedb），这个数据库中含有本地所有文件信息。

Linux系统自动创建这个数据库（/var/lib/locatedb），并且每天自动更新一次，所以使用locate命令查不到最新变动过的文件。

$ locate <指定目录>

## whereis

whereis命令只能用于程序名的搜索，而且只搜索二进制文件（参数-b）、man说明文件（参数-m）和源代码文件（参数-s）。如果省略参数，则返回所有信息。

$ whereis <参数> <二进制文件名>

## which

which命令的作用是，在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果。

使用which命令，就可以看到某个系统命令是否存在，以及执行的到底是哪一个位置的命令。

$ which <命令名>

## type

type用来区分某个命令到底是由shell自带的，还是由shell外部的独立二进制文件提供的。

使用-p参数，会显示该命令的路径

$ type <参数> <命令名>

## grep

查找过滤文本

### 常用参数

-v(invert-match)

-c(count)

-n(line-number)

-i(ignore-case)

-l, -L, -R(-r, –recursive), -e

## awk

### 基本参数

NR 行号, NF 列数量

$1 第1列, $2, $3…

-F fs fs分隔符，字符串或正则

### 语法

awk 'BEGIN{ commands } pattern{ commands } END{ commands }'

### 流程

执行begin

对输入每一行执行 pattern{ commands }, pattern 可以是 正则/reg exp/, 关系运算等

处理完毕, 执行 end

## sed

文本替换

### 常用参数

-d 删除

-s 替换, g 全局

-e 多个命令叠加

-i 修改原文件(Mac下加参数 “”, 备份)

## cut

按列取数据，awk也可以

### 常用参数

-b(字节)

-c(字符)

-f(第几列), -d(分隔符), f范围: n, n-, -m, n-m

## sort

排序

### 常用参数

-d, –dictionary-order

-n, –numeric-sort

-r, –reverse

-b, –ignore-leading-blanks

-k, –key

## uniq

一般和 sort 一块用, 只能去重相邻的行

### 常用参数

-c 重复次数

-d 重复的

-u 没重复的

-f 忽略前几列

## diff

比较文件

## paste

两个文件按列拼接

### 常用参数

-d 分隔符

-s 列转行

## ps

**ps**是**Process Status**的缩写，用来列出系统中当前运行的那些进程。**ps**命令列出的是当前进程的快照，就是执行**ps**命令这个时刻的进程，可以使用**top**命令获取动态的进程信息。

### 常用命令参考

显示所有进程  ps -A

显示指定用户进程  ps -u [username]

显示所有进程，包括命令行提示符信息 ps -ef

显示所有正在内存中进程，展示进程占用系统资源信息  ps -aux

ps与grep组合使用，查找cmd匹配指定内容的进程  ps -aux|grep php

列出命令行相关的进程  ps -l

树状结构展示所有的进程 ps -axjf

显示进程信息，并记录到指定文件中(指定文件名不存在则默认创建该文件)  ps -aux > log.txt

## netstat

netstat 命令用于显示各种网络相关信息，如网络连接，路由表，接口状态(Interface Statistics),masquerade连接，多播成员(Multicast Memberships)等

命令的输出有两部分：

第一部分是Active Internet connections，称为有源TCP连接，其中"Recv-Q"和"Send-Q"指的是接收队列和发送队列，这些数字一般都应该是0，如果不是则表示软件包正在队列中堆积。


第二部分是Active UNIX domain sockets，称为有源Unix域套接口(和网络套接字一样，但是只能用于本机通信，性能比网络套接字高一倍)。Proto显示连接使用的协议,RefCnt表示连接到本套接口
上的进程号,Types显示套接口的类型,State显示套接口当前的状态,Path表示连接到套接口的其它进程使用的路径名。

### 命令参数

netstat命令默认是不显示LISTEN状态的网络连接和LISTEING状态的UNIX域连接，只有使用带-a或者-l参数的命令才能显示出来。

-a (all)显示所有状态的连接

-t (tcp)仅显示tcp相关连接

-u (udp)仅显示udp相关连接

-n 拒绝显示别名，能显示数字的全部转化成数字

-l 仅列出有在监听状态的连接

-p 显示建立相关链接的程序名

-r 显示路由信息，路由表

-e 显示扩展信息，例如uid等

-s 按各个协议进行统计

-c 每隔一个固定时间，执行该netstat命令

### 常用命令

列出所有连接（包括监听和未监听状态）

命令：netstat -a；列出所有状态下的连接

命令：netstat -at；列出所有状态下的tcp连接

命令：netstat -au；列出所有状态下的udp连接

命令：netstat -ax；列出所有状态下的UNINX域连接

列出处于LISTEN状态的连接

命令：netstat -l；列出所有处于LISTEN状态的连接

命令：netstat -lt；列出所有处于LISTEN状态的tcp连接

命令：netstat -lu；列出所有处于LISTEN状态的udp连接

命令：netstat-lx；列出所有处于LISTENING状态的UNIX域连接

统计通信协议连接信息

命令：netstat -s；统计所有连接的通信协议连接信息

命令：netstat -st；统计基于tcp连接的通信协议连接信息

命令：netstat -su；统计基于udp连接的通信协议连接信息

输出中显示进程ID和进程名信息（可搭配其他参数使用）

命令：netstat -p；列出除LISTEN和LISTENING状态下的连接，包含连接所属进程的进程ID和进程名

命令：netstat -tp；列出除LISTEN和LISTENING状态下的tcp连接，包含连接所属进程的进程ID和进程名

命令：netstat -up；列出除LISTEN和LISTENING状态下的udp连接，包含连接所属进程的进程ID和进程名

动态输出连接信息

命令：netstat -c；每间隔一秒输出当前连接信息

列出特定的连接

命令：netstat -ap|grep postgres