# Git 的基本使用

[Git websit](https://git-scm.com/)

## 前言

**Git**是一套分散式的版本管理系统，版本控制是一个开发团队中不可或缺的工具。

## 安装

**Git**的安装方法有很多，也有很多**GUI**工具，但个人还是推荐使用命令行界面，因为对于程序员，**命令行**是不可能回避的一个领域，所以不要逃避。

* [Mac setup](https://help.github.com/articles/set-up-git/)

* [Windows setup](https://help.github.com/articles/set-up-git/#platform-windows)

* [Linux setup](https://help.github.com/articles/set-up-git/#platform-linux)

## 工作流

你的本地仓库由**git**维护的三棵“树”组成。第一个是你的**工作目录**，它持有实际文件；第二个是**缓存区（Index）**，它像个缓存区域，临时保存你的改动；最后是**HEAD**，指向你最近一次提交后的结果。

![git workflow](/img/git_2.png)

## Git命令

### git config

配置项都会被**Git**存储到家目录下的**.gitconfig**文档中

### git init

建立一个新的**Repository**

### git clone [Repository url] [local repository name]

通过给出地址**Clone**一个**Repository**，并根据需要是否指定本地**Repository**名

**git clone**支持多种协议，除了**HTTP(s)**以外，还支持**SSH**、**Git**、本地文件协议

### git status

在一个**Git**的**Repository**中，你可以输入`git status`来检查目前**Git**的状态

### git add

当有了新增计划改动，**Git**会提示有**Untracked files**未被追踪档案，之后需要使用`git add`来告诉**Git**开始追踪它。

没有修改过的文件称为**unstage**，追踪后的状态为**stage**

### git commit

已经在**stage**下的文件下一步就是**commit**，以实际提交改动到**HEAD**。
每一个**commit**在**Git**中就是一个节点，这些节点就是将来你可以回顾和追查的参考。

* **git commit --amend**如果当次提交后发现漏掉了几个文件没有添加，或者提交信息写错了。可以继续操作文件到缓存区中，然后执行`git commit --amend`，追加缓存区的文件到上一个commit中

### git reset

取消git动作

* **git reset HEAD [file]**取消stage暂存的档案(撤销`git add`)

* **git checkout -- [file]**取消修改过的档案(撤销`git add`和修改)

* **git reset --hard ORIG_HEAD**取消上次merge操作

### git remote

远程仓库是指托管在因特网或其他网络中的你的项目的版本库，每个项目可以有好几个远程仓库

此命令用于管理主机名

* 使用**-v**选项，可以参看远程主机的网址

* 使用**-o**指定远程主机名

* **git remote show <主机名>**可以查看该主机的详细信息

* **git remote add <主机名> <网址>**用于添加远程主机

* **git remote rm <主机名>**用于删除远程主机

* **git remote rename <原主机名> <新主机名>**用于远程主机的改名

### git fetch

```bash
git fetch [remote-name]
```

这个命令会访问远程仓库，从中拉取所有你还没有的数据。执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。

**git fetch**命令会将数据拉取到你的本地仓库，但它并不会自动合并或修改你当前的工作。当准备好时你必须手动将其合并入你的工作。

* **git fetch <远程主机名>**将某个远程主机的更新全部取回本地

* **git fetch <远程主机名> <分支名>**将某个远程主机的某个分支的更新取回本地

### git pull

* **git pull <远程主机名> <远程分支名>:<本地分支名>**取回远程主机某个分支的更新，再与本地的指定分支合并

* 如果当前分支与远程分支存在追踪关系，**git pull**就可以省略远程分支名。

### git push

* **git push <远程主机名> [[<本地分支名>:]<远程分支名>]**将本地仓库**HEAD**中的更新推送到远程仓库

* 如果省略远程分支名，则表示将本地分支推送与之存在"追踪关系"的远程分支（通常两者同名），如果该远程分支不存在，则会被新建

* 如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支

* 如果当前分支与远程分支存在追踪关系，则本地分支和远程分支都可以省略，将当前分支推送到origin主机的对应分支

* 如果当前分支只有一个追踪分支，那么主机名都可以省略。

* **git branch --set-upstream <本地分支> <远程分支名>**手动建立追踪关系

### git branch

git分支管理操作

* **git branch**列出所有本地branch并告诉你当前所在的branch

* **git branch -r**查看远端分支

* **git branch -a**查看所有分支，本地+远端

* **git branch --no-merged**查看本地未合并分支

* **git branch -merged**查看本地已合并分支，应该被删除

* **git branch <分支名>**使用你定义的分支名创建一个新的branch

* **git branch -d <分支名>**删除本地分支(必须是已合并分支)

* **git branch -D <分支名>**强制删除本地分支

* **git checkout -b <新分支> <远端分支>**创建分支

相当于

```bash
git branch <新分支>
git checkout <新分支>
```

* **git checkout <分支名>**切换分支

### git merge

* **git merge <远端分支>**合并分支

### git rebase

将提交到某一分支上的所有修改都移至另一分支上

原理是首先找到这两个分支的最近共同祖先，然后对比当前分支相对于该祖先的历次提交，
提取相应的修改并存为临时文件，然后将当前分支指向目标基底, 最后以此将之前另存为临时文件的修改依序应用。

* **git rebase <分支名>**简单说就是整理本地分支，将分支上已提交的commit打包成一个新的commit，然后将当前分支重置为新建状态，里面只包含这个新的commit提交

虽然rebase很有用，但**请不要对在你的仓库外有副本的分支执行变基！！！**

### git log

回顾提交历史

默认不用任何参数的话，git log会按提交时间列出所有的更新，最近的更新排在最上面。
这个命令会列出每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明。

* **git log -n**仅显示最近的 n 条提交

* **git log --[since/after]=[time]**仅显示指定时间之后的提交。

* **git log --[until/before]=[time]**仅显示指定时间之前的提交。

* **git log --grep=[value]**仅显示含指定关键字的提交

* **git log --author=[name]**仅显示指定作者相关的提交。

* **git log --grep=[value]**仅显示含指定关键字的提交

* **git log -S[function_name]**仅显示添加或移除了某个关键字的提交

* **git log -p**显示每次提交的内容差异，该选项除了显示基本信息之外，还在附带了每次 commit 的变化

* **git log --stat**显示每次提交的简略的统计信息。列出额所有被修改过的文件、有多少文件被修改了以及被修改 过的文件的哪些行被移除或是添加了。

* **git log --pretty=[value]**指定使用不同于默认格式的方式展示提交历史。

* **git log --pretty=online**指定使用单行展示提交历史。

* **git log --pretty=short**指定使用简略方式展示提交历史。显示提交者和commit信息

* **git log --pretty=fuller**指定使用不同于默认格式的方式展示提交历史。显示提交者和commit信息，提交时间等信息

* **git log --pretty=format:"%h - %an, %ar : %s"**定制要显示的提交历史记录格式，方便对后期提取分析

|选项 |说明|
|----|---------|
| %H | 提交对象(commit)的完整哈希字串 |
| %h | 提交对象的简短哈希字串 |
| %T | 树对象(tree)的完整哈希字串 |
| %t | 树对象的简短哈希字串 |
| %P | 父对象(parent)的完整哈希字串 |
| %p | 父对象的简短哈希字串 |
| %an | 作者(author)的名字 |
| %ae | 作者的电子邮件地址 |
| %ad | 作者修订日期(可以用 --date= 选项定制格式) |
| %ar | 作者修订日期，按多久以前的方式显示 |
| %cn | 提交者(committer)的名字 |
| %ce | 提交者的电子邮件地址 |
| %cd | 提交日期 |
| %cr | 提交日期，按多久以前的方式显示 |
| %s | 提交说明 |

* **git log --pretty=format:"%h %s" --graph**图像展现分支，合并历史

...To be continued