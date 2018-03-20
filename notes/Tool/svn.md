# SVN

## 命令

### svn checkout

svn checkout URL[@REV]... [PATH]
检出

### svn status

svn status [PATH...]
输出WC中文件和目录的状态。如果WC提交，这些状态就会同步到库里

* ' '  没有修改
* 'A'  新增
* 'D'  删除
* 'M'  修改
* 'R'  替代
* 'C'  冲突
* 'I'  忽略
* '?'  未受控
* '!'  丢失，一般是将受控文件直接删除导致

### svn update

svn update [PATH...]
更新WC，更新反馈有如下几种分类

* A  新增
* B  锁破坏
* D  删除
* U  更新
* C  冲突
* G  合并
* E  存在的

### svn add

svn add [path]
添加文件或目录到你的wc，打上新增标记。这些文件会在下一次你提交wc的时候提交到svn服务器

### svn revert

svn revert [path]
撤销新增的文件

### svn commit

svn commit [PATH...]
把你WC的更改提交到仓库，默认情况下提交必须提供log message，参数**--message|-m**

### svn resolve

svn resolve PATH...
将冲突的文件标记为已解决，并且删掉冲突产生的临时文件。注意这个命令并不是能把冲突解决，解决冲突还是得靠人工
加上**--accept**参数，尝试自动处理冲突

已弃用svn resolved PATH...

### svn revert

svn revert PATH...
还原WC中所有的本地更改，加上**--depth=infinity**将还原整个目录

### svn diff

svn diff [-r rev1[:rev2]] [path]
用来比较并显示修改点，**-r**添加版本比较，**path**添加比较路径

### svn log

svn log [PATH]
从库中显示log消息。log消息代码

* A ：added
* D：deleted
* M：modified
* R：replaced

### svn mkdir

svn mkdir [PATH|URL]
在WC或库路径创建目录

### svn move

svn move SRC... DST
等同于svn copy命令跟个svn delete命令。WC到URL的重命名是不被允许的

### svn switch

svn switch URL[@PEGREV] [PATH]
将WC转向一个其他的库地址同步

### svn cat

svn cat TARGET[@REV]
输出指定目标的内容，这里的目标一般是文件，加上**--version n**显示版本号为**n**的**path**内容

### svn cleanup

svn cleanup [PATH...]
递归的清理WC中过期的锁和未完成的操作

### svn copy

svn copy SRC[@REV]... DST
copy操作可以从WC到WC；WC到URL；URL到WC；URL到URL。现在SVN只支持同一个仓库内文件的拷贝，不允许跨仓库操作

copy命令是创建分支和1标记的常用方式。copy到url的操作隐含了提交动作，所以需要提供log messages

### svn merge

svn merge
合并两个受控源的不同之处，存放到一个WC里

svn merge sourceURL1[@N] sourceURL2[@M] [WCPATH]

svn merge sourceWCPATH1@N sourceWCPATH2@M [WCPATH]

svn merge [[-c M]... | [-r N:M]...] [SOURCE[@REV] [WCPATH]]

### svn delete

svn delete PATH...
删除

### svn info

svn info [target]
显示指定WC和URL信息

### svn list

svn list [TARGET[@REV]...]
显示目标下的文件和目录列表

### svn import

svn import [PATH] URL
导入本地一个目录到库中。但是导入后，本地的目录并不会处于受控状态

### svn export

svn export [-r REV] [URL|PATH][@PEGREV] [PATH]
导出一个干净的目录树，不包含所有的受控信息。可以选择从URL或WC中导出

### svn lock

svn lock TARGET...
对目标获得修改锁。如果目标已被其他用户锁定，则会抛出警告信息

### svn unlock

svn unlock TARGET...
对目标获得解锁

### svn blame

svn blame Target[@REV]
显示某个已受控文件的每一行的最后修改版本和作者，加上**--xml**参数可以以xml格式显示每一行的属性
