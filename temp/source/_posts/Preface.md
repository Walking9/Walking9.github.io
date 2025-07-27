---
title: Preface
date: 2017-12-21 12:38:26
tags: [conclusion, Hexo, Github, Markdown]
---
## 前言

昨天，浏览了学校大四学长的博客以及github，一共200多篇博文，github上面的项目也都做的非常好，通过这位netcan666学长的博文中可以大概看出他大学四年的经历，确实十分精彩，现已拿到华为上海研究所的offer。深有感触，现为自己在此立下flag。

我现在已经大三了，之前大二的时候也弄过博客以及github，但属于三天打渔二十天晒网的类型，也并不是说大二之前的时间荒废了，没有做事，而是并没有养成坚持写博客，开源useful的github的项目，而是没有自我总结的习惯，有些事我做过的了就做过了，而不会去记录，去反思自己学到了什么，我认为只是一种非常不好的习惯。所以，我从昨天开始在linux下搭建Hexo博客，关联github，抛弃以前的github和csdn上的博客，从新开始，This is a new begining.

## Hexo + Github（Ubuntu16.04下）

[参考该链接](http://www.jianshu.com/p/a3ab83dba041)

### 安装node

`curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -`

`sudo apt-get install -y nodejs`

安装之后用node -v检查node版本，看是否安装成功，这里我认为原链接是有一处错误的，因为我们安装的是nodejs，而没有安装node，所以还需要执行

`sudo apt-get install -y node`

否则后续安装工作不能正常进行

### 安装git

因为以前我是用过所以这一步我跳过，但不介意也把命令贴出来

`sudo apt install git`

`git --version`

### 注册github账号

[以下摘自该链接](http://blog.csdn.net/binyao02123202/article/details/20130891)

我将我以前github账号上的项目clone下来，删除账号！

新注册的账号需要配置ssh key，步骤：

1、先检查是否已经已经存在ssh key:

`$ cd ~/.ssh`

`$ ls`

这两个命令就是检查是否已经存在 id_rsa.pub 或 id_dsa.pub 文件，如果文件已经存在，那么你可以跳过步骤2，直接进入步骤3

2、创建一个ssh key

`$ ssh-keygen -t rsa -C "your_email@example.com"`

代码参数含义：

-t 指定密钥类型，默认是 rsa ，可以省略。
-C 设置注释文字，比如邮箱。
-f 指定密钥文件存储文件名。

以上代码省略了 -f 参数，因此，运行上面那条命令后会让你输入一个文件名，用于保存刚才生成的 SSH key 代码，如：

```
Generating public/private rsa key pair.
# Enter file in which to save the key (/c/Users/you/.ssh/id_rsa): [Press enter]
```

接着又会提示你输入两次密码（该密码是你push文件的时候要输入的密码，而不是github管理者的密码），

当然，你也可以不输入密码，直接按回车。那么push的时候就不需要输入密码，直接提交到github上了，如：

```
Enter passphrase (empty for no passphrase): 
# Enter same passphrase again:
```

接下来，就会显示如下代码提示，如：

```
Your identification has been saved in /c/Users/you/.ssh/id_rsa.
# Your public key has been saved in /c/Users/you/.ssh/id_rsa.pub.
# The key fingerprint is:
# 01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@example.com
```

当你看到上面这段代码的收，那就说明，你的 SSH key 已经创建成功，你只需要添加到github的SSH key上就可以了。

3、添加你的 SSH key 到 github上面去

a、首先你需要拷贝 id_rsa.pub 文件的内容，你可以用编辑器打开文件复制，也可以用git命令复制该文件的内容，如：

```
$ clip < ~/.ssh/id_rsa.pub
```

b、登录你的github账号，从又上角的设置（ [Account Settings](https://github.com/settings) ）进入，然后点击菜单栏的 SSH key 进入页面添加 SSH key。

c、点击 Add SSH key 按钮添加一个 SSH key 。把你复制的 SSH key 代码粘贴到 key 所对应的输入框中，记得 SSH key 代码的前后不要留有空格或者回车。当然，上面的 Title 所对应的输入框你也可以输入一个该 SSH key 显示在 github 上的一个别名。默认的会使用你的邮件名称。

4、测试一下该SSH key

在git Bash 中输入以下代码

```
$ ssh -T git@github.com
```

当你输入以上代码时，会有一段警告代码，如：

```
The authenticity of host 'github.com (207.97.227.239)' can't be established.
# RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
# Are you sure you want to continue connecting (yes/no)?
```

这是正常的，你输入 yes 回车既可。如果你创建 SSH key 的时候设置了密码，接下来就会提示你输入密码，如：

```
Enter passphrase for key '/c/Users/Administrator/.ssh/id_rsa':
```

当然如果你密码输错了，会再要求你输入，知道对了为止。

注意：输入密码时如果输错一个字就会不正确，使用删除键是无法更正的。

密码正确后你会看到下面这段话，如：

```
Hi username! You've successfully authenticated, but GitHub does not
# provide shell access.
```

如果用户名是正确的,你已经成功设置SSH密钥。如果你看到 "access denied" ，者表示拒绝访问，那么你就需要使用 https 去访问，而不是 SSH 。

### 正式安装Hexo

[Hexo 中文网站](https://hexo.io/zh-cn/)

`npm install hexo-cli -g`

安装 `Hexo` 完成后，请执行下列命令，Hexo将会在指定文件夹中新建所需要的文件。

`hexo init blog`

Tips ：blog是你新建的文件夹，blog 的位置就是 Hexo工作空间

`cd blog`

进入Hexo工作空间，执行

`npm install`

检查Hexo版本，检查是否安装成功

`hexo -v`

至此hexo已安装成功！启动Hexo服务

`hexo server`

在浏览器中输入localhost:4000，就可以看到一个全新的你的个人博客(Tips : 如果 4000 端口被占用，hexo server -p 5000)

### github与本地Hexo建立连接

1、在 github 建立 Repository

建立与你用户名对应的仓库，仓库名必须为【your_user_name.github.io】，这是固定写法

2、在站点的配置文件`_config.yml`中设置部署信息

Tips:`_config.yml`位置 hexo 工作空间的目录下

[官网上对该文件有详细介绍](https://hexo.io/zh-cn/docs/configuration.html)

```
deploy:
  type: git
  repo: git@github.com:YiShanQingF/YiShanQingF.github.io.git
  branch: master
  message: '站点更新:{{now("YYYY-MM-DD HH:mm:ss")}}'
```

然后执行下面命令

`npm install hexo-deployer-git --save`

Tips：注意格式，`：`后有空格，`type` `repo` `branch` `message`前有两个空格

3、部署，三条命令：

`hexo clean`

`hexo generate`

`hexo deploy`

然后在浏览器中输入 [http://GitName.github.io](https://localhost:4000) 就行了，根据自己 github 的账户，将GitName换成自己的就OK了！

### 绑定域名

分为三步：购买域名、绑定域名、绑定设置。由于我暂时没有考虑将自己的博客上传到网络，所以这一步暂时不做。

[参考链接](http://www.jianshu.com/p/a3ab83dba041)

## Markdown

Hexo 支持Markdown语法，熟练使用Markdown写文章也是非常有用的。

[Markdown官方文档](http://wowubuntu.com/markdown/)

我对于Markdown也是有过接触的，以前使用csdn博客的时候用的就是Markdown编辑器，但是使用的还是不够熟练，这里找到了一个比较不错的Markdown新手教程，日后还需要多加练习：

[Markdown新手教程](http://www.jianshu.com/p/q81RER)

### Ubuntu下Vim + Markdown

效果图：

![Alt text](https://upload-images.jianshu.io/upload_images/2582258-2189b6e80f5fa7e5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

参考这个链接我也做到了同样的效果，期间并未遇到棘手的问题，所以直接贴出链接

[Vim+Markdown配置](https://www.jianshu.com/p/44d31327f953)

### Typora

> Typora是一个功能强大的Markdown编辑器，使用GFM风格（即大名鼎鼎的github flavored markdown），Typora目前支持Mac OS和Windows，Linux版本尚未发布。Typora可以插入数学表达式，插入表情，表格，支持标准的Markdown语法。

安装过程参考自：

[How to Install Typora – A Minimal Markdown Editor in Ubuntu](http://ubuntuhandbook.org/index.php/2016/09/install-typora-minimal-markdown-editor/)

三个命令完成安装：

1、Add Typora Linux repository via command:

`sudo add-apt-repository 'deb https://typora.io linux/`

2、Setup the key:

`sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE`

3、Finally update and install this simple markdown editor:

`sudo apt update`

`sudo apt install typora`

Using:

![Alt text](/images/Typora_using.png)

​

## Other

解决Hexo上不能显示本机图片问题：

> 1.首先确认_config.yml 中有 post_asset_folder:true。
Hexo 提供了一种更方便管理 Asset 的设定：post_asset_folder,当设置post_asset_folder为true参数后，在建立文件时，Hexo会自动建立一个与文章同名的文件夹，可以把与该文章相关的所有资源都放到那个文件夹，如此一来，便可以更方便的使用资源。
2.在hexo的目录下执行npm install https://github.com/CodeFalling/hexo-asset-image –save（需要等待一段时间）。
3.完成安装后用hexo新建文章的时候会发现_posts目录下面会多出一个和文章名字一样的文件夹。图片就可以放在文件夹下面。

## This is a new begining:

![Alt text](/images/music.jpg)