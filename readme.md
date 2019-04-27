#  一、git简介

不同于SVN这种集中式版本管理系统，Git是Linux之父Linus用c语言开发的的一个分布式版本管理系统。

## git是分布式的，svn是集中式的

这是git和svn两者最大的区别，先说集中式版本控制系统，版本库是集中放在中央服务器，而工作的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始工作，工作结束后在推送给中央服务器。集中式版本控制系统最大的毛病就是必须联网才能工作。分布式版本管理系统没有中央服务器，每个人的电脑都是一个完整的版本库，这样的话，在工作的时候就不需要联网。



# 二、安装Git并创建版本库

## 1.安装Git

输入`git --version`查看系统有没有安装`git`

```$ git —version```

如果要在 Linux 上安装预编译好的 Git 二进制安装包，可以直接用系统提供的包管理工具。在 Fedora 上用 yum 安装：

```$ yum install git-core```

在 Ubuntu 这类 Debian 体系的系统上，可以用 apt-get 安装：

```$ apt-get install git```

[参考](https://git-scm.com/book/zh/v1/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)

安装完成后，输入`git —version`查看Git的版本

```
$ git --version
git version 1.8.3.1
```

## 2.配置Git

提交文件到仓库时，Git需要知道你的身份，需要在本地配置你的username和email

```shell
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```



## 3.创建版本库

版本库又叫做仓库(repository),可以简单理解成一个目录，这个目录下的所有文件都会被Git管理起来，每个文件的修改，删除以及添加都会被Git追踪到，以便任何时刻都可以追踪历史或者将来的某个时刻可以还原。

创建一个版本库非常简单，首先创建一个空目录

```shell
$ mkdir git_test

$ cd git_test/
```

注意在window系统下，避免全路径目录名不要包含中文，否则可能会出现各种莫名其妙的问题

第二步，通过 `git init`命令把这个目录变成Git可以管理的仓库

```
$ git init
初始化空的 Git 版本库于 /root/git_test/.git/
```

这时就已经创建好了一个空的Git仓库，你会发现当前目录下多了一个`.git`的目录。（linux系统下`.git`可能会隐藏，`ls -ah`可以查看。）

## 4.添加文件到版本库

首先需要明确一点，所有的版本控制系统都只能追踪文本文件的改动，比如txt文件，网页，程序代码等，Git也不例外。比如在第5行添加了一个单词Linux，在第8行删除了一个单词Window。而图片，视频这些二进制文件，虽然也能由版本控制系统管理，但没法追踪文件的变化，只能把这些二进制文件每次改动串起来，也就是知道图片从100kb改成120kb，但具体改了啥，版本控制系统不知道。

Git在windows系统使用时会出现的坑：Microsoft的Word是二进制格式，因此版本控制系统没法追踪Word文件的改动。不要使用windows自带的记事本编辑任何文本文件，因为Microsoft开发记事本的团队使用了一个非常弱智的行为来保存UTF-8编码的文件，他们自作聪明地在每个文件开头添加了0xefbbbf（十六进制）的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个“?”，明明正确的程序一编译就报语法错误，等等。

同时建议所有文本文件都按照`UTF-8` 编码，所有语言使用同一种编码，既没有冲突，又被所有平台所支持。

回归正题，我们在刚才创建的目录下新建一个文件readme.txt文件,内容如下：

```txt
Git is a version control system.
Git is free software.
```

第一步，用命令`git add`告诉Git把文件添加到仓库

```shell
$ git add readme.txt
```

第二步，用命令`git commit`告诉Git，将文件提交到仓库

```shell
$ git commit -m 'add readme.txt file'
[master（根提交） 55bea42] add readme.txt file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

简单解释一下`git commit`命令，`-m`后面输入本次提交的说明，最好不要是没有意义的内容

`git commit -m <message>`执行成功后，会告诉你，`1 file changed`一个文件被改动，`2 insertions(+)`插入了两行内容

为什么Git添加文件需要`add`，`commit`一共两步呢？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件，比如：

```shell
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```



# 三、版本库管理

## 1.版本回退

先对readme.txt文件进行两次版本修改

版本一： add readme file

```
Git is a version control system.
Git is free software.
```

版本二：add distributed

```
Git is a distributed version control system.
Git is free software.
```

版本三：append GPL

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

实际工作中我们没法记住文件的每一次改动细节，Git提供`git log`命令来让我们查看提交的历史记录

```shell
$ git log
commit 282fd0e5dcc94fdf0eab50cdc1a615fa75e13b94
Author: root <916032798@qq.com>
Date:   Thu Apr 25 13:41:29 2019 +0800

    append GPL

commit 2853651242f100d0c3815dc30164a6aae3f9574c
Author: root <916032798@qq.com>
Date:   Thu Apr 25 13:40:38 2019 +0800

    add distributed

commit 55bea423cb8cbd8dce315f0959a03ee09300c0ea
Author: root <916032798@qq.com>
Date:   Thu Apr 25 12:06:05 2019 +0800

    add readme.txt file
```

`git log`命令显示了从最近到最远的提交日志，我们可以看到3次的提交记录，最近一次是`append GPL`,最早一次是`add readme.txt file`

如果嫌输出信息太多，则可以加上`—pretty=oneline`

```
$ git log --pretty=oneline
282fd0e5dcc94fdf0eab50cdc1a615fa75e13b94 append GPL
2853651242f100d0c3815dc30164a6aae3f9574c add distributed
55bea423cb8cbd8dce315f0959a03ee09300c0ea add readme.txt file
```

前面一串十六进制的数字是`commit id`(版本号)，是由SHA1计算出来的一个很大的数字

每提交一个新版本，实际上Git就会把它们自动串成一条时间线。

现在如果我们想把readme.txt回退到上个版本`add distributed`那个版本，需要怎么做呢？

首先，Git必须知道当前版本是那个版本，在Git中，用`HEAD`表示当前版本，也就是最新提交的版本`282fd0e5dcc94fdf0eab50cdc1a615fa75e13b94`，上个版本就是`HEAD^`，上上个版本`HEAD^^`，当版本数变多时可以这样表述，如前100个版本`HEAD~100`

现在，我们要把当前版本`append GPL`回退到`add distributed`可以使用`git reset`命令

```shell
$ git reset --hard HEAD^
HEAD 现在位于 2853651 add distributed
```

查看readme的内容，发现果然回退到 add distributed这个版本

```shell
Git is a distributed version control system.
Git is free software.
```

现在用`git log`查看版本库的状态

```shell
$ git log
commit 2853651242f100d0c3815dc30164a6aae3f9574c
Author: root <916032798@qq.com>
Date:   Thu Apr 25 13:40:38 2019 +0800

    add distributed

commit 55bea423cb8cbd8dce315f0959a03ee09300c0ea
Author: root <916032798@qq.com>
Date:   Thu Apr 25 12:06:05 2019 +0800

    add readme.txt file

```

发现最新的版本`append GPL`已经看不到了。好比你从21世纪坐时光穿梭机来到了19世纪，想再回去已经回不去了。这是如果我们又想回到`append GPL`这个版本该怎么办呢？

如果你的命令行界面没有关，这是可以看到`appedd GPL`这个版本的`commit id 282fd0e5dcc94fdf0eab50cdc1a615fa75e13b94`,这时就可以通过`commit id`回到未来的版本

```shell
$ git reset --hard 282fd0e5
HEAD 现在位于 282fd0e append GPL
```

版本号可以不用写全，Git会自动查找对应的版本，不过写的不能过少导致找到的版本号不唯一

查看readme文件的内容发现又回到了`append GPL`这个版本

```
Git is a distributed version control system.
Git is free software under the GPL.
```

Git的版本回退速度非常快，因为Git在内部有个指向当前版本的`HEAD`指针，当回退版本时，Git只是将`HEAD`指向了你要回退的版本

```
|HEAD│
   │
   └──> ○ append GPL
        │
        ○ add distributed
        │
        ○ add readme.txt file
```

改为指向`add diatributed`:

```
│HEAD│
   │
   │    ○ append GPL
   │    │
   └──> ○ add distributed
        │
        ○ add readme.txt file
```

如果了回退到了某一个版本，关闭电脑第二天又想到最新的版本`append GPL`，又找不到`commit id`怎么办呢

Git提供了一个命令`git -reflog`来记录你的每次命令

```shell
$ git reflog
282fd0e HEAD@{0}: reset: moving to 282fd0e5
2853651 HEAD@{1}: reset: moving to HEAD^
282fd0e HEAD@{2}: commit: append GPL
2853651 HEAD@{3}: commit: add distributed
55bea42 HEAD@{4}: commit (initial): add readme.txt file
```

从输出的信息看到，现在又可以看到`append GPL`的`commit id`了，这样又可以根据版本号回退到未来的版本了

## 2.工作区和暂存区

### 工作区

就是你在电脑中能看到的目录，比如git_test目录就是一个工作区

### 版本库

git_test下有个隐藏目录`.git`就是Git的版本库。

Git的版本库中存了很多东西，其中最重要的就是称为stage的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`

前面讲了把文件往Git版本库中添加的时候，分两步执行

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建一个`master`分支，所以，现在`git commit`就是往`master`分支上提交更改。可以简单的理解为。需要提交的文件修改通通放在暂存区，然后一次性提交暂存区的所有修改。

现在我们先对readme.txt做个修改

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
```

然后在工作区中新增一个`LICENSE`文件

先用`git status`查看一下状态

```shell
$ git status
# 位于分支 master
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      readme.txt
#
# 未跟踪的文件:
#   （使用 "git add <file>..." 以包含要提交的内容）
#
#	LICENSE
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

Git非常清楚地告诉我们，readme.txt被修改了，而`LICENSE`还没有被添加过，它的状态是`Untracked`的

现在用`git add`提交`readme.txt`和`LICENSE`这两个文件

```shell
$ git add readme.txt LICENSE
```

`git status`查看状态

```shell
$ git status
# 位于分支 master
# 要提交的变更：
#   （使用 "git reset HEAD <file>..." 撤出暂存区）
#
#	新文件：    LICENSE
#	修改：      readme.txt
#
```

所以，`git add`命令实际上就是把要提交的所有修改放到暂存区，然后，执行`git commit`就可以一次性的把暂存区里的所有内容提交到分支上。

```shell
$ git commit -m 'understand how stage works'
[master 412631a] understand how stage works
 2 files changed, 2 insertions(+)
 create mode 100644 LICENSE
```

一旦提交后，如果你对工作区没做任何修改，那么工作区和暂存区都是""干净""的

```shell
$ git status
# 位于分支 master
无文件要提交，干净的工作区
```

## 3.管理修改

为什么Git会比其他版本控制系统设计的优秀，因为Git跟踪并管理的是修改，而非文件。对文件的”CRUD“均属于修改。

我们可以做个试验来理解为什么说Git管理的是修改而不是文件

先对readme.txt文件增添一行

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
```

然后，添加：

```shell
$ git add readme.txt
$ git status
# 位于分支 master
# 要提交的变更：
#   （使用 "git reset HEAD <file>..." 撤出暂存区）
#
#	修改：      readme.txt
#
```

然后，在修改readme.txt

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
```

提交：

```shell
$ git commit -m 'git tracks changes'
[master e3abc7d] git tracks changes
 1 file changed, 1 insertion(+)
```

提交后，再看看状态：

```shell
$ git status
# 位于分支 master
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      readme.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

发现第二次的修改没有被提交，用`git diff HEAD -- readme.txt`可以查看工作区和版本库里面最新版本的区别

```shell
$ git diff HEAD -- readme.txt
diff --git a/readme.txt b/readme.txt
index d5d432e..c917e49 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,4 +1,4 @@
 Git is a distributed version control system.
 Git is free software under the GPL.
 Git has a mutable index called stage.
-Git tracks changes.
+Git tracks changes of files.
```

可见第二次修改确实没有被提交。所以可以看出Git管理的是修改而不是文件。

## 4.撤销修改

假设你现在往readme.txt添加了一行内容

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
My stupid boss still prefers SVN.
```

在提交前，突然想到要删除添加的最后一行，把文件恢复到上一个版本，用`git status`查看状态如下

```shell
$ git status
# 位于分支 master
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      readme.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

发现输出的信息中Git告诉你`git checkout -- <file>...`可以丢弃工作区的改动

命令`git checkout -- readme.txt`的意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况

一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态

一种是`readme.txt`已经添加到暂存区后，又做了修改，现在，撤销修改就回到添加到暂存区后的状态

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

查看`readme.txt`的内容，发现文件内容复原了

```
Git is a distributed version control system.
Git is free software under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
```

`git checkout --file`命令中的`—`很重要，没有`—`，就变成了"切换到另一个分支的命令“。

假设你把修改内容`git add`到了暂存区

```shell
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
My stupid boss still prefers SVN.

$ git add readme.txt
```

`git status`查看一下

```shell
$ git status
# 位于分支 master
# 要提交的变更：
#   （使用 "git reset HEAD <file>..." 撤出暂存区）
#
#	修改：      readme.txt
#
```

从输出信息中Git告诉我们，用命令`git reset HEAD <file>可以把暂存区的修改撤销掉，重新放回到工作区`

```shell
$ git reset HEAD readme.txt
重置后撤出暂存区的变更：
M	readme.txt
```

`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本.

再用`git status`查看一下

```shell
$ git status
# 位于分支 master
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      readme.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

然后用`git checkout -- readme.txt`就可以完全丢弃对工作区的修改了

```shell
$ git checkout -- readme.txt
$ cat readme.txt
Git is a distributed version control system.
Git is free software under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
$ git status
# 位于分支 master
无文件要提交，干净的工作区
```

假设你不仅把修改的内容提交到暂存区，还提交到了版本库，这时就需要用到我们前面学的版本回退的命令`git reset --hard HEAD^`

## 5.删除文件

在Git中，删除也是一个修改操作，我们先添加一个新文件`test.txt`到Git并且提交

```shell
$ git add test.txt
$ git commit -m 'add test.txt'
[master f05b885] add test.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 test.txt
```

然后删除文件

```shell
rm -f test.txt
```

`git status`查看工作区状态

```shell
# 位于分支 master
# 尚未暂存以备提交的变更：
#   （使用 "git add/rm <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	删除：      test.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

这个时候你有两个选择，一是确实要从版本库中删除test.txt文件，只需要用`git rm`命令，并提交`git commit`

```shell
$ git rm test.txt
rm 'test.txt'
$ git commit -m 'remove test.txt'
[master 9b44b75] remove test.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 test.txt
```

先手动删除文件，然后使用`git rm <file>`和`git add <file>`效果是一样的。

现在文件就从版本库中被删除了

另一种情况是删错了，那么我们需要把文件恢复到版本库中的最新版本中，这时就可以用到之前学到的命令`git checkout -- test.txt`

`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以"一键还原"



# 四、远程仓库

前面我们学了如何在一个仓库中管理文件历史，现在如果是团队多人协作或者担心某一天硬盘挂掉的情况，这时就需要用到Git的远程仓库了。

后面我会介绍如何利用GitLab搭建一个Git服务器作为远程仓库，现在可以借助[GitHub](<https://github.com/>)这个基于Git的全球最大的代码托管网站来学习Git远程仓库的使用。

由于本地Git仓库和github仓库之间的传输是通过SSH加密的，所以需要先进行设置：

第一步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

```shell
$ ssh-keygen -t rsa -C "youremail@example.com"
```

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可,密码可以不需要设置

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

第二步：登录github，打开`setting`页面，在`SSH keys`这一栏添加`id_rsa.pub`你的公钥

为什么github需要SHH Key呢？因为github需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以github只要知道了你的公钥，就可以确认只有你自己才能推送。

## 1. 添加远程仓库

首先在github上创建一个空的`learngit`仓库，在本地的`learngit`仓库下运行命令

```shell
$ git remote add origin git@github.com:kuzan1994/learngit.git
```

添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库

下一步，就可以把本地库的所有内容推送到远程库上：

```shell
$ git push -u origin master
Enter passphrase for key '/Users/sui/.ssh/id_rsa': 
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 7.10 KiB | 7.10 MiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:kuzan1994/learngit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

从现在起只要本地做了提交，就可以通过命令：

```shell
$ git push origin master
```

将本地`master`分支的最新修改提交推送到github上去。

## 2.从远程库克隆

前面我们介绍了先有本地库，后有远程库，然后在关联远程库的情况。

现在，假设我们从零开发一个新项目，那么最好的方式是先创建远程库，然后从远程库克隆。

首先我们在github上创建一个新的远程仓库`gitskill`

```shell
$ git clone git@github.com:kuzan1994/gitskill.git
Cloning into 'gitskill'...
Enter passphrase for key '/Users/sui/.ssh/id_rsa': 
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
```

我们对本地仓库中的`README.md`做修改并提交

```shell
$ vim README.md
$ git add README.md
$ git commit -m 'modify README'
$ git push -u origin master
```

因为本地仓库本身就是从远程仓库克隆的，所以不需要在关联远程仓库。

同时我们可以注意到，github给出的地址不止一个，还可以用`https://github.com/kuzan1994/gitskill.git`这样的地址，实际上，Git支持多种协议，默认使用`git://`使用SSH，但也可以使用`https`,使用`https`速度会慢一些。但是在某些只开放http端口的公司内部就无法使用`ssh`协议而只能用`https`。



# 五、分支管理

大多数的版本控制系统也有分支管理的功能，不过它们采取的是备份所有项目文件到指定的目录，所以根据项目文件数量和大小不同，可能花费的时间也会有相当大的差别，快则几秒，慢则几分钟，而Git分支的本质是一个指向`commit`对象的一个指针,所以Git分支的创建和销毁会非常廉价和迅速。

## 1.创建与合并分支

在前面的学习中我们知道Git把所有的提交串成一条时间线，这条时间线就是一个分支。截止到目前为止，只有一条时间线，在Git中这个分支叫做主分支，即`master`分支，`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。

![](https://github.com/kuzan1994/learngit/blob/master/images/1.png)

现在我们先创建一个`dev`分支

```shell
$ git branch dev
```

这个时候Git会创建一个指向最新提交的一个指针

![](https://github.com/kuzan1994/learngit/blob/master/images/2.png)

`git branch`命令创建分支时，不会把当前分支切换到新分支，所以当前分支还是`master`分支，`HEAD`还是指向`master`

使用`git checkout <branch>`切换到指定分支

```shell
$ git checkout dev
切换到分支 'dev'
```

![](https://github.com/kuzan1994/learngit/blob/master/images/3.jpg)

`git checkout`命令加上`-b`参数表示创建并切换，相当于一下两条命令

```shell
$ git branch dev
$ git checkout dev
```

然后，用`git branch`命令查看当前分支

```shell
$ git branch
* dev
  master
```

`git branch`命令会列出所有分支，当前分支前面会标一个*号

我们在`dev`分支上对readme.txt做修改添加一行

```
Creating a new branch is quick.
```

然后提交：

```shell
$ git add readme.txt
$ git commit -m 'branch test'
[dev 38486ad] branch test
 1 file changed, 1 insertion(+)
```

![](https://github.com/kuzan1994/learngit/blob/master/images/4.jpg)

现在我们切换回`master`分支查看`readme.txt`的内容

```shell
$ git checkout master
切换到分支 'master'
$ cat readme.txt
Git is a distributed version control system.
Git is free software under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
```

发现刚才添加的内容不见了！因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并没有变：

![](https://github.com/kuzan1994/learngit/blob/master/images/5.jpg)

现在我们把`dev`分支上修改的内容合并到`master`上改如何做呢？

```shell
$ git merge dev
更新 9b44b75..38486ad
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
$ cat readme.txt
Git is a distributed version control system.
Git is free software under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
Creating a new branch is quick.
```

![](https://github.com/kuzan1994/learngit/blob/master/images/6.jpg)

`git merge`命令用于合并指定分支到当前分支。合并后，在查看`readme.txt`的内容发现，和`dev`分支修改的完全一样。

注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是`快进模式`，就是直接把`master`指向`dev`当前提交，所以合并速度会非常快。后面会学习到其他合并方式。

如果要删除`dev`分支`git branch -d <branch-name>`

```shell
$ git branch -d dev
已删除分支 dev（曾为 38486ad）。
$ git branch
* master
```

删除后我们发现现在只剩下了`master`主分支

## 2.解决冲突

使用Git做团队协作工具的同学一定都会遇到过，你把本地代码推送到远程仓库或者合并分支时，无法推送，提示冲突的情况。

先准备一个新的`feature1`分支

```shell
$ git checkout -b feature1
切换到一个新分支 'feature1'
```

修改`readme.txt`最后一行，改为：

```
Creating a new branch is quick AND simple.
```

在`feature1`分支上提交

```shell
$ git add readme.txt
git commit -m 'AND simple'
[feature1 05bfb3b] AND simple
 1 file changed, 1 insertion(+), 1 deletion(-)
```

切换到`master`分支：

```shell
$ git checkout master
```

在`master`分支上把`readme.txt`文件的最后一行改为：

```
Creating a new branch is quick & simple.
```

提交：

```shell
$ git add readme.txt 
$ git commit -m '& simple'
[master 5d176b3] & simple
 1 file changed, 1 insertion(+), 1 deletion(-)
```

现在，`master`分支和`feature1`分支各自都分别有新的提交，变成了这样：

![](https://github.com/kuzan1994/learngit/blob/master/images/7.png)

这个时候，Git无法执行"快速合并"，只能试图把各自的修改合并起来，但是这种合并可能会引起冲突，试一下

```shell
$ git merge feature1
自动合并 readme.txt
冲突（内容）：合并冲突于 readme.txt
自动合并失败，修正冲突然后提交修正的结果。
```

果然冲突了，Git告诉我们，`readme.txt`文件存在冲突，必须手动解决冲突后在提交。`git status`也可以告诉我们冲突的文件：

```shell
$ git status
# 位于分支 master
# 您有尚未合并的路径。
#   （解决冲突并运行 "git commit"）
#
# 未合并的路径：
#   （使用 "git add <file>..." 标记解决方案）
#
#	双方修改：     readme.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

查看`readme.txt`里面的内容

```
$ cat readme.txt
Git is a distributed version control system.
Git is free software under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```

Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，我们修改如下后保存：

```
Creating a new branch is quick and simple.
```

提交：

```shell
$ git add readme.txt 
$ git commit -m 'comflict fixed'
[master ac502c8] comflict fixed
```

现在`master`分支和`feature1`分支变成如下所示

![](https://github.com/kuzan1994/learngit/blob/master/images/8.png)

用带参数的`git log`也可以看到分支的合并情况

```shell
$ git log --graph --pretty=oneline --abbrev-commit
*   ac502c8 comflict fixed
|\  
| * 05bfb3b AND simple
* | 5d176b3 & simple
|/  
* 38486ad branch test
* 9b44b75 remove test.txt
* f05b885 add test.txt
* 80f9788 track changes
* e3abc7d git tracks changes
* 412631a understand how stage works
* 282fd0e append GPL
* 2853651 add distributed
* 55bea42 add readme.txt file
```

最后，删除`feature1`分支

```shell
$ git branch -d feature1
已删除分支 feature1（曾为 05bfb3b）。
```

## 3.分支管理策略

通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在merage时生成一个新的commit,这样，从分支历史上就可以看出分支信息。

下面我们使用`--no-ff`方式的`git merge`

先创建并切换分支`dev`

```shell
$ git checkout -b dev
切换到一个新分支 'dev'
```

修改`readme.txt`文件并提交

```shell
$ git add readme.txt 
$ git commit -m "add merge"
[dev f52c633] add merge
 1 file changed, 1 insertion(+)
```

现在，切换回`master`分支

合并`dev`分支，注意`--no-ff`参数，表示禁用`Fast forword`

```shell
$ git checkout master
$ git merge --no-ff -m 'merge with no-ff' dev
Merge made by the 'recursive' strategy.
 readme.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

因为本次合并要创建一个新的commit,所以加上`-m`参数，把commit描述写进去。

合并后，我们可以用`git log`看看分支历史

```shell
$ git log --graph --pretty=oneline --abbrev-commit
*   cb78c7c merge with no-ff
|\  
| * 41476f3 add merge
|/  
*   ac502c8 comflict fixed
|\  
| * 05bfb3b AND simple
* | 5d176b3 & simple
|/  
* 38486ad branch test
* 9b44b75 remove test.txt
* f05b885 add test.txt
* 80f9788 track changes
* e3abc7d git tracks changes
* 412631a understand how stage works
* 282fd0e append GPL
* 2853651 add distributed
* 55bea42 add readme.txt file
```

可以看到，不使用`Fast forward`模式，merge后就像这样：

![](https://github.com/kuzan1994/learngit/blob/master/images/9.png)

### 分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活

干活都在`dev`分支，也就是说，`dev`分支是不稳定的，到某个时候，比如比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

## 4.bug分支

现在假设我们在`dev`分支上对`readme.txt`内容进行了修改，此时我们并不想提交修改，同时`readme.txt`上有个bug,需要我们紧急修复。现在我们自然想到要创建一个`issue-101`来修复它，但是，当前`dev`分支上的工作还没有提交。

```shell
$ git status
# 位于分支 dev
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      readme.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

Git提供了一个`stash`功能，可以把当前工作现场"储藏"起来，等以后恢复现场后继续工作

```shell
$ git stash
Saved working directory and index state WIP on dev: 41476f3 add merge
HEAD 现在位于 41476f3 add merge
```

用`git status`查看工作区，是干净的。

假设我们想在`master`上修复，即在`master`上创建临时bug分支

```shell
$ git checkout master
切换到分支 'master'
$ git checkout -b issue-101
切换到一个新分支 'issue-101'
```

现在修复bug，需要把“Git is free software ...”改为“Git is a free software ...”，然后提交：

```shell
$ git add readme.txt 
$ git commit -m 'fix bug 101'
[issue-101 a42f7e4] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

现在切换回`master`上，合并分支

```shell
$ git checkout master
$ git merge --no-ff -m 'merge bug fix 101' issue-101
Merge made by the 'recursive' strategy.
 readme.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git branch -d issue-101
已删除分支 issue-101（曾为 a42f7e4）。
```

现在我们修复了`master`上的bug,是时候切换回`dev`分支上继续干活了

```shell
$ git checkout dev
切换到分支 'dev'
$ git status
# 位于分支 dev
无文件要提交，干净的工作区
```

现在工作区是干净的，那么怎么恢复刚才我们"储藏"的内容呢，用`git stash list`看看

```shell
$ git stash list
stash@{0}: WIP on dev: 41476f3 add merge
```

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用`git stash appply`恢复，但恢复后，stash内容并不删除，你需要用`git stash drop`来删除

另一种方式是用`git stash pop`，恢复的同时把stash内容也删了

```shell
$ git stash pop
# 位于分支 dev
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      readme.txt
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
丢弃了 refs/stash@{0} (da1113b5c1664f177e58ed21dc296a05b55f3cdc)
```

查看`readme.txt`的内容发现又恢复过来了

再用`git stash list`查看，就看不到任何stash内容了：

```shell
$ git stash list
```

你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：

```shell
$ git stash apply stash@{0}
```

## 5.Feature分支

相信同学们在工作中一定都遇到过这个情况，突然接到一个新需求要做一个新功能，做到一半突然又说这个功能舍弃掉。在做这个新功能的时候为了不打扰主分支的功能，你会选择新建一个新的临时分支，当听到功能不做的时候，这时候临时分支就不会合并到主分支上，要删掉这个还没有被合并的临时分支改怎么做呢。我们先用`git branch -d <branch name>`试一下可不可以删除掉。

```shell
$ git checkout -b feature
切换到一个新分支 'feature'
$ touch hello.txt
$ git add hello.txt
$ git commit -m 'new feature add hello.txt'
[feature 4541d87] new feature add hello.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 hello.txt
$ git checkout master
error: 分支 'feature' 没有完全合并。
如果您确认要删除它，执行 'git branch -D feature'。
```

发现Git提示`feature`分支没有完全合并，如果想要删除，执行`git branch -D feature`

```shell
$ git branch -D feature
$ git branch
  dev
* master
```

发现`feature`分支确实删除掉了

## 6.多人协作

当你要从远程仓库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来，并且，远程仓库的默认名称是`origin`。

要查看远程库的信息，用`git remote`

```shell
$ git remote
origin
```

或者用`git remote -v`显示更详细的信息

```shell
$ git remote -v
origin	git@github.com:kuzan1994/learngit.git (fetch)
origin	git@github.com:kuzan1994/learngit.git (push)
```

上面显示了抓取和推送的`origin`地址。如果没有推送权限，就看不到push的地址

### 推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

```shell
$ git push origin master
```

如果要推送其他分支，比如`dev`，就改成

```shell
$ git push origin dev
```

但是,并不是一定要把本地分支往远程推送，那么哪些分支需要推送，哪些不需要

- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

### 抓取分支

多人协作时，大家都会往`master`和`dev`分支上推送各自的修改

现在，模拟一个你的小伙伴，可以在另一台电脑（注意要把SSH Key添加到GitHub）或者同一台电脑的另一个目录下克隆：

```shell
$ git clone git@github.com:kuzan1994/learngit.git
Cloning into 'learngit'...
Enter passphrase for key '/Users/sui/.ssh/id_rsa': 
remote: Enumerating objects: 24, done.
remote: Counting objects: 100% (24/24), done.
remote: Compressing objects: 100% (17/17), done.
remote: Total 24 (delta 4), reused 24 (delta 4), pack-reused 0
Receiving objects: 100% (24/24), 121.62 KiB | 155.00 KiB/s, done.
Resolving deltas: 100% (4/4), done.
```

当你的小伙伴从远程库clone时，默认情况下，你的小伙伴只能看到本地的`master`分支。不信可以用`git branch`命令看看

```shell
$ git branch
* master
```

```shell
$ $ git checkout -b dev origin/dev
```

现在，他就可以在`dev`上继续修改，然后，时不时地把`dev`分支`push`到远程：

```shell
$ git add env.txt

$ git commit -m "add env"
[dev 7a5e5dd] add env
 1 file changed, 1 insertion(+)
 create mode 100644 env.txt

$ git push origin dev
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 308 bytes | 308.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
   f52c633..7a5e5dd  dev -> dev
```

你的小伙伴已经向`origin/dev`分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送：

```shell
$ cat env.txt
env
$ git add env.txt 
$ git commit -m 'add new env'
[dev aabec0b] add new env
 1 file changed, 1 insertion(+)
 create mode 100644 env.txt
$ git push origin dev
To github.com:kuzan1994/learngit.git
 ! [rejected]        dev -> dev (non-fast-forward)
error: failed to push some refs to 'git@github.com:kuzan1994/learngit.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送：

```shell
$ git pull
From github.com:kuzan1994/learngit
   81294fb..9d1c0b4  dev        -> origin/dev
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> dev
```

`git pull`也失败了，原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示，设置`dev`和`origin/dev`的链接：

```shell
$ git branch --set-upstream-to=origin/dev dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```

这回`git pull`成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的[解决冲突](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840202368c74be33fbd884e71b570f2cc3c0d1dcf000)完全一样。解决后，提交，再push：

```shell
$ git pull
```

这回`git pull`成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的[解决冲突](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840202368c74be33fbd884e71b570f2cc3c0d1dcf000)完全一样。解决后，提交，再push：

```shell
$ git commit -m 'fix env conflict'
$ git push origin dev
Counting objects: 4, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (4/4), 465 bytes | 465.00 KiB/s, done.
Total 4 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), done.
To github.com:kuzan1994/learngit.git
   9d1c0b4..77e8007  dev -> dev
```

因此，多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。



# 六、标签管理

Git的标签相当于版本库的快照，实际上就是指向某个commit的指针(和分支很像对不对，但是分支可以移动，标签不能移动)。所以创建和删除标签都是瞬间完成的。Git既然有commit了为什么还要引入tag呢，想象一下，如果你接到领导的一个要求把commit号是9d1c0b4...打包发布一下，这时一大串数字是不是很难记住，如果说把版本号是v1.1的发布，这就很容易记了。所以tag就是一个让人容易记住的有意义的名字，它是和某个commit绑定在一起的。

## 1.创建标签

在Git中打标签非常简单，首先，切换到需要打标签的分支上：

```shell
$ git checkout master
```

然后，使用命令`git tag <name>`就可以打一个新的标签：

```shell
$ git tag v1.0
```

可以用`git tag`查看所有标签

```shell
$ git tag
v1.0
```

默认标签是打在最新提交的commit上的，如果想给以前提交的commit打标签又该怎么做呢

方法就是找到历史的commit id，然后打上就好了

```shell
$ git log --pretty=oneline --abbrev-commit
*   4b189f6 merge bug fix 101
|\  
| * a42f7e4 fix bug 101
|/  
*   cb78c7c merge with no-ff
|\  
| * 41476f3 add merge
|/  
*   ac502c8 comflict fixed
|\  
| * 05bfb3b AND simple
* | 5d176b3 & simple
|/  
* 38486ad branch test
* 9b44b75 remove test.txt
* f05b885 add test.txt
* 80f9788 track changes
* e3abc7d git tracks changes
* 412631a understand how stage works
* 282fd0e append GPL
* 2853651 add distributed
* 55bea42 add readme.txt file
```

比方说要对`add merge`这次提交打标签，它对应的commit id是`41476f3`，敲入命令：

```shell
$ git tag v0.9 41476f3
```

再用命令`git tag`查看标签：

```shell
$ git tag
v0.9
v1.0
```

注意，标签不是按时间顺序列出，而是按字母排序的。可以用`git show <tagname>`查看标签信息：

```shell
$ git show v0.9
commit 41476f32d6986de0ee51d52a087b29f2ea6b7e0e
Author: root <916032798@qq.com>
Date:   Fri Apr 26 15:47:24 2019 +0800

    add merge

diff --git a/readme.txt b/readme.txt
index 01be954..1f07ebf 100644
--- a/readme.txt
+++ b/readme.txt
@@ -2,4 +2,4 @@ Git is a distributed version control system.
 Git is free software under the GPL.
 Git has a mutable index called stage.
 Git tracks changes of files.
-Creating a new branch is quick and simple.
+Creating a new branch is quick and simple add merge.
```

可以看到，`v0.9`确实打在`add merge`这次提交上

还可以创建带有说明的标签，用`-a`指定标签名，用`-m`指定说明文字

```shell
$ git tag -a v0.1 -m 'version 0.1 released' 282fd0e
```

用命令`git show <tagname>`可以看到说明文字：

```shell
$ git show v0.1
tag v0.1
Tagger: root <916032798@qq.com>
Date:   Sat Apr 27 11:07:18 2019 +0800

version 0.1 released

commit 282fd0e5dcc94fdf0eab50cdc1a615fa75e13b94
Author: root <916032798@qq.com>
Date:   Thu Apr 25 13:41:29 2019 +0800

    append GPL
    ....
```

注意：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。

## 2.操作标签

如果标签打错了，也可以删除：

```shell
$ git tag -d v0.1
已删除 tag 'v0.1'（曾为 701e092）
```

因为创建的标签都之存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

如果要推送某个标签到远程，使用命令`git push origin <tagname>`

```shell
$ git push origin v0.1
Total 0 (delta 0), reused 0 (delta 0)
To github.com:kuzan1994/learngit.git
 * [new tag]         v0.1 -> v0.1
```

或者，一次性推送全部尚未推送到远程的本地标签：

```shell
$ git push origin --tags
Total 0 (delta 0), reused 0 (delta 0)
To github.com:kuzan1994/learngit.git
 * [new tag]         v0.2 -> v0.2
```

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

```shell
$ git tag -d v0.2
Deleted tag 'v0.2' (was b967dd9)
```

然后，从远程删除，删除命令也是`push`：

```shell
$ git push origin :refs/tags/v0.2
To github.com:kuzan1994/learngit.git
 - [deleted]         v0.2
```

去github上查看发现v0.2的tag确实已经删除



# 七、往github上的开源项目提交代码

不重复造轮子是很多程序员认可的软件开发理念，github上有很多热门的开源项目供我们参考和工作使用。很多小伙伴是不是也想参与那些热门的开源项目呢。那么我们如何参与开源项目中呢。

首先需要访问你参与的项目，在它的主页有`fork`按钮，点击后就fork到你的github仓库下

拿我的开源项目[learngit](<https://github.com/kuzan1994/learngit>)举例

![](https://github.com/kuzan1994/learngit/blob/master/images/10.jpg)

然后去你的账号下面就可以看到在你仓库下面已经有了`learngit`项目了，接着`clone`到你本地上进行修改

```shell
git clone git@github.com:<YourGithubName>/learngit.git
```

在本地你可以写一些你自己的Git学习和使用心得，然后推送到自己github远程库上。

然后如果你想要我的项目中加上你自己的内容的话，就需要在github上`pull request`

![](https://github.com/kuzan1994/learngit/blob/master/images/10.jpg)

这样你就可以把你的`pull request`推送给我了，在我的项目下就能看到你的`pull request了，至于我会不会接受就要看心情了。

好了，至此我们就可以通过这个流程了解到了如何参与github上别人的开源项目了。

