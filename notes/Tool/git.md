# Git 的基本使用

[Git websit](https://git-scm.com/)

## 前言
**Git**是一套分散式的版本管理系统，版本控制是一个开发团队中不可或缺的工具。
**Workflow**对于使用**Git**有非常大的影响，一个好的**Workflow**会使你很快学习**Git**。

这是一篇**Git**的快速上手

## 安装
**Git**的安装方法有很多，也有很多**GUI**工具，但个人还是推荐使用命令行界面，因为身为程序员**命令行**是不可能回避的一个领域，所以不要逃避。

  * [Mac setup](https://help.github.com/articles/set-up-git/)
  * [Windows setup](https://help.github.com/articles/set-up-git/#platform-windows)
  * [Linux setup](https://help.github.com/articles/set-up-git/#platform-linux)

*因为我使用的是**Mac**，所以以下在没有特别说明的情况下图片都来自**Mac OS**系统*

##Git配置

在使用**Git**之前，先不要着急去想着使用它。因为**Git**在每一次**commit**时都会记录作者的信息，我们先来把你的信息设定好

```
$ git config --global user.name "zhaolei"
$ git config --global user.email "leozhao.go@gmail.com"
```

加上`--global`表示是全局的设定。你可以使用`git config --list`查看当前git的配置列表。

```
$ git config --list
```

其实你的配置项都会被**Git**存储到你家目录下的**.gitconfig**文档中

```
$ cat ~/.gitconfig
```

你可以看到文件你的已配置好的配置项，同理，你也可以对.gitconfig进行修改，添加配置项，和上面命令行操作是等同的，配置好退出就好，之后**Git**会自动重载。

使用终端来操作**Git**常会让人觉得一直打指令很繁琐，好在**Git**提供了**alias**功能，你可以这样设定：

```
$ git config --global alias.st status
```

之后你需要使用`git status`，可以输入`git st`了。

## 开始使用
要开始使用**Git**你必须先建立一个**Git**的**Repository**，有两种方法可以建立**Repository**：

  * `git init`    建立一个新的**Repository**
  * `git clone [Repository url] [local repository name]`   **Clone**一个别人的**Repository**

## 基本命令

### status
在一个**Git**的**Repository**中，你可以输入`git status`来检查目前**Git**的状态

```
$ git status
# On branch master
#
# Initial commit
#
nothing to commit (create/copy files and use "git add" to track)
```

看到上面的系统提示，代表我们在一个干净的目录，**On branch master**表示当前的**branch**为**master**

### add
当有了新增文件后，**Git**会提示有**Untracked files**未被追踪档案，之后需要使用`git add`来告诉**Git**开始追踪它。
没有修改过的文件称为**unstage**，追踪后的状态为**stage**

### commit

已经在**stage**下的文件下一步就是**commit**。每一个**commit**在**Git**中就是一个节点，这些节点就是将来你可以回顾和追查的参考。

```
$ git commit -m "Add test.rb to test git function"
```

一定要加`-m`参数并简洁、清楚的填入当前这次**commit**的目的，这会让你之后在查找节点时带来便利。

### log

当你希望查看过往节点的时候，可以这样

```
$ git log
5f76371 - (HEAD, master) add ignore files (3 minutes ago)
4ca268d - (origin/master) First commit (33 minutes ago)
```

前面的代表**commit**版本号，后面的是你输入的**commit**目的、时间以及提交者，也可以在后面添加`--stat`查看详细信息


...To be continued







