# Git 的基本使用

[Git websit](https://git-scm.com/)

## 前言
**Git**是一套分散式的版本管理系统，版本控制是一个开发团队中不可或缺的工具。

## 安装
**Git**的安装方法有很多，也有很多**GUI**工具，但个人还是推荐使用命令行界面，因为身为程序员**命令行**是不可能回避的一个领域，所以不要逃避。

  * [Mac setup](https://help.github.com/articles/set-up-git/)
  * [Windows setup](https://help.github.com/articles/set-up-git/#platform-windows)
  * [Linux setup](https://help.github.com/articles/set-up-git/#platform-linux)

## Git命令

### git config

配置项都会被**Git**存储到你家目录下的**.gitconfig**文档中

### git init

建立一个新的**Repository**

### git clone [Repository url] [local repository name]

通过给出地址**Clone**一个**Repository**，并根据需要是否指定本地**Repository**名

**git clone**支持多种协议，除了**HTTP(s)**以外，还支持**SSH**、**Git**、本地文件协议

### git remote

用于管理主机名

* 使用**-v**选项，可以参看远程主机的网址

* 使用**-o**指定远程主机名

* **git remote show <主机名>**可以查看该主机的详细信息

* **git remote add <主机名> <网址>**用于添加远程主机

* **git remote rm <主机名>**用于删除远程主机

* **git remote rename <原主机名> <新主机名>**用于远程主机的改名

### git fetch

远程主机的版本库有了comment，将这些更新取回本地

* **git fetch <远程主机名>**将某个远程主机的更新全部取回本地

* **git fetch <远程主机名> <分支名>**将某个远程主机的某个分支的更新取回本地

* **-r**查看远程分支

* **-a**查看所有分支

### git pull

* **git pull <远程主机名> <远程分支名>:<本地分支名>**取回远程主机某个分支的更新，再与本地的指定分支合并

* 如果当前分支与远程分支存在追踪关系，**git pull**就可以省略远程分支名。

### git push

* **git push <远程主机名> <本地分支名>:<远程分支名>**将本地分支的更新，推送到远程主机

* 如果省略远程分支名，则表示将本地分支推送与之存在"追踪关系"的远程分支（通常两者同名），如果该远程分支不存在，则会被新建

* 如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支

* 如果当前分支只有一个追踪分支，那么主机名都可以省略。
* **git branch --set-upstream <本地分支> <远程分支名>**手动建立追踪关系

### git branch

git分支操作

* **git branch**列出所有branch并告诉你当前所在的branch

* **git branch <分支名>**使用你定义的名字创建一个新的branch

* **git checkout -b <新分支> <远端分支>**创建分支

* **git checkout <分支名>**切换分支

* **git rebase**整理本地分支，当远端有新的comment时

* **git merge <远端分支>**合并分支

### git reset

取消git动作

* **git reset --hard ORIG_HEAD**取消上次merge操作

* **git reset HEAD <file>**取消stage暂存的档案

* **git checkout -- <file>**取消修改过的档案

* **git commit --amend**修改之前commit信息

### git status

在一个**Git**的**Repository**中，你可以输入`git status`来检查目前**Git**的状态

### git add

当有了新增文件后，**Git**会提示有**Untracked files**未被追踪档案，之后需要使用`git add`来告诉**Git**开始追踪它。

没有修改过的文件称为**unstage**，追踪后的状态为**stage**

### git commit

已经在**stage**下的文件下一步就是**commit**。每一个**commit**在**Git**中就是一个节点，这些节点就是将来你可以回顾和追查的参考。

### git log


...To be continued







