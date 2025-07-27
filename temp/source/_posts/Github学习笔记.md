---
title: Github学习笔记
date: 2017-12-23 14:31:16
tags: [github]
---

# 前言

对于使用github一知半解的状态是十分难受的，遇到问题时需要不断的查如何解决，所以还不如学习一下github的原理，这才是解决问题的根源。

一开始使用github的时候肯定看过类似github新手教程之类的博客，但由于没有丰富的知识基础为底，导致使用github起来非常不顺手，抽个空学了一下github的原理，并将所理解的记录下来。

# github的发展

> 这是Git：
>
> Git是一款免费、开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。
>
> Git是一个开源的分布式版本控制系统，可以有效、高速的处理从很小到非常大的项目版本管理。Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。
>
> 这是GitHub：
>
> gitHub是一个面向开源及私有软件项目的托管平台，因为只支持git 作为唯一的版本库格式进行托管，故名gitHub。
>
> gitHub于2008年4月10日正式上线，除了git代码仓库托管及基本的 Web管理界面以外，还提供了订阅、讨论组、文本渲染、在线文件编辑器、协作图谱（报表）、代码片段分享（Gist）等功能。目前，其注册用户已经超过350万，托管版本数量也是非常之多，其中不乏知名开源项目 Ruby on Rails、jQuery、python 等。
>
> –以上来自"百度百科"

以下参考自：[Git和GitHub原理讲解及区别](http://blog.csdn.net/lifuxiangcaohui/article/details/17216733)

在没有接触github之前，做项目时我们也肯定不会将之前写过的版本删除，而是做个备份，用复制整个项目目录的方式来保存不同的版本，或许还会改名加上备份时间进行区别。这么做的唯一好处就是简单，坏处也不少：有时候会混淆所在的工作目录，一旦弄错了文件数据就没办法撤销恢复。反思下自己，确实是这样啊！

1、为了解决这个问题，人们很久以前就开发了许多本地版本控制系统，大多是采用某种简单的数据库来记录文件的历次更新差异。Like this:

![本地的版本控制系统](/images/1.png)

2、问题：如何让在不同系统上的开发者协同工作？

解决：集中化的版本控制系统

![集中化的版本控制系统](/images/2.png)

3、再问题：中央服务器的单点故障(中央服务器完蛋了就真完蛋了。。。)

解决：分布式版本控制系统

![分布式版本控制系统](/images/3.png)

分布式版本控制系统在 **是否关心文件内容的问题上**分成了Git系统和非Git系统(CVS、Subversion等):

Git只关心文件数据的整体是否发生变化，而大多数其他系统则只关心文件内容的具体差异。这类系统（CVS、Subversion等）每次记录都有哪些文件做了更新，以及更新了哪些行的什么内容。

![一般](/images/4.png)

VS

![Git](/images/5.png)

# github初步理解与使用

在学习了网上许多博客并结合自己亲自实践之后，对github有了初步了解，在此我将用自己的语言，通俗易懂的记录下来。

![git](/images/6.png)

图片来自：

http://blog.csdn.net/u012575819/article/details/50553501

现在，我有一个以及完成注册的账号，并且已经完成了ssh配置，需要在github上创建一个自己的项目。

## 创建Workspace

在自己电脑上，选择一个位置建立文件夹Project作为自己的workspace，所以有关项目的所有内容都需要在该workspace内完成，此时查看git状态：

`$ git status`

是没有任何内容的：

```
brunon@brunon-ThinkPad-E450:~$ git status
位于分支 master

初始提交

未跟踪的文件:
  （使用 "git add <文件>..." 以包含要提交的内容）
....(省略)
```

要对现有的该Project开始用 Git 管理，只需**到此项目所在的目录**，执行：

`$ git init`

初始化后，在当前目录下会出现一个名为 .git 的目录，所有 Git 需要的数据和资源都存放在这个目录中。在ubuntu下肉眼是看不到.git的存在，因为该目录是隐藏的，需要使用命令 ls -a在终端中找到了该目录的存在。这个.git目录可以对应理解成上图中的Repository，它作为了你本地项目和远程主机对接的中间人。

## 对Project进行跟踪

在项目workspace下，也就是Project这个文件，里面有自己的项目文件，此时查询git的状态，就会发现Project内的项目文件处于未被跟踪的状态。

补充：文件的三种状态

对于任何一个文件，在 Git 内都只有三种状态：已提交（committed），已修改（modified）和已暂存（staged）。已提交表示该文件已经被安全地保存在本地数据库 中了；已修改表示修改了某个文件，但还没有提交保存；已暂存表示把已修改的文件放在下次提交时要保存的清单中。由此我们看到 Git 管理项目时，文件流转的三个工作区域：Git 的工作目录，暂存区域，以及本地仓库。

这是在Git内的文件三种状态，而Project内的项目文件还不属于Git内的文件，所以需要使用

`$ git add projectFile`

(假定Project内有个projectFile的项目文件)

则此时该projectFile文件就处于了已暂存staged态，类比图中index。

此时已经完成了对该文件的跟踪，当该文件发生变化时就会被记录下来

## 提交

我的理解是：已暂存**staged**态的文件和已修改**modified**的文件需要被提交到本地数据库中，使用

`$ git commit -m "提交的描述信息"`

这一步，通俗的将就是我已经做好了对文件的编辑、修改了，并确认无误，提交到本地数据库进行存档，等我其他文件也做好并存档了，就可以进行最后一步了：将我所完成的整个项目推送到远程主机上。

## Push

`git push`命令用于将本地分支的更新，推送到远程主机上；

首先，需要将本地仓库与github仓库关联 （如果没有关联的话），这一步我的理解是，如果一个团队在共同完成一个项目，例如这个项目的远程端仓库名为Common，团队的项目当然是共同完成的，所以需要把我的本地仓库与github仓库关联，否则github不知道需要推送的哪个仓库上(备注：github仓库提前建好)。

`$ git remote add origin https://github.com/你的github用户名/你的github仓库.git`

这样，在使用`git push` 命令，就可以完成本地仓库到远程主机的推送了(备注：使用该命令会有提示需要输入个人的github账号和密码)：

`$ git push origin master`

最后，在浏览器上查看个人github就可以看到提交的内容了。

# END

个人使用github的基本操作差不多就这些，需要用到其他操作及命令查一查就可以解决，理解了github大致工作原理，这些也将变得简单了，以后在团队合作完成项目时，再学学进一步的使用，能实践才是最好的。

系列学习：

http://www.open-open.com/lib/view/open1328069733264.html

参考链接：

http://blog.csdn.net/tina_ttl/article/details/51326684

http://blog.csdn.net/u012575819/article/details/50553501

http://blog.csdn.net/lifuxiangcaohui/article/details/17216733

--2017.12.23

# 继续学习github

前几天完成了数据库课程设计，与朋友一起完成的这次设计，就是用的github做版本控制，真的发现github太太太赞了，经朋友推荐[廖雪峰github讲解](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)写的非常好，现在继续学习一下，并结合自己的使用经验，拿起笔记记录下来。

## 版本回退功能

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。
- （突然发现这是markdown语法，直接把他的小结复制下来了^V^）

## 工作区和暂存区

我认为这张图片已经说明了一切。。。

![工作区和暂存区](/images/7.png)

（工作区—-项目文件夹； 版本库—-.git目录）

- 用`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别
- `git checkout -- file`可以丢弃工作区的修改，就是让这个文件回到最近一次`git commit`或`git add`时的状态。
- 用命令`git reset HEAD file`可以把暂存区的修改撤销掉（unstage），重新放回工作区。

## 远程仓库

- 关联远程仓库

在本地的`learngit`仓库下运行命令：

```
$ git remote add origin git@github.com:Username/RepositoryName.git
```

本地仓库与远程仓库关联。把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

```
$ git push -u origin master
```

而取消关联:

`$ git remote remove origin`

- clone一个仓库：

```
$ git clone git@github.com:Username/RepositoryName.git
```

要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

Git支持多种协议，包括`https`，但通过`ssh`支持的原生`git`协议速度最快。

## 分支

- 创建与合并分支

关于分支的使用，作者写的很容易理解：[创建与合并分支](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840038939c291467cc7c747b1810aab2fb8863508000)

![图示](/images/8.png)

图示等价于：

```
master
*dev
```

Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

- 解决冲突

现在有两个这样的分支：

![](/images/9.png)

此时需要手动解决两个分支的冲突（如果在一条线上的两个分支，合并分支的话会自动解决冲突）

Git用`<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，我们修改后保存再提交：

![](/images/10.png)

用`git log --graph`命令可以看到分支合并图。

- bug分支和feature分支

根据作者所述，结合自己的理解，记录如下：

![](/images/11.png)

## 多人协作

​    在理清了分支的基础上，多人协作就是建立在这之上的进一步应用，由于数据库课程设计就是与朋友协作完成的，虽然我不太会用，但通过实践还是很有收获的，故余虽愚，足或有所闻，哈哈。

## 标签

创建标签：

tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。

- 命令`git tag <name>`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；
- `git tag -a <tagname> -m "blablabla..."`可以指定标签信息；
- `git tag -s <tagname> -m "blablabla..."`可以用PGP签名标签；
- 命令`git tag`可以查看所有标签。

操作标签

- 命令`git push origin <tagname>`可以推送一个本地标签；
- 命令`git push origin --tags`可以推送全部未推送过的本地标签；
- 命令`git tag -d <tagname>`可以删除一个本地标签；
- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

实践出真知，以后还得多用才行，忘了就翻笔记！

--2018.1.14