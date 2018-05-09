---
title: Hexo + GitHub Pages 搭建博客
date: 2018-03-29 10:56:09
tags:
---

------

大概流程：

 1. 搭建 Node.js 环境
 2. 搭建 Git 环境
 3. GitHub配置
 4. 安装配置 Hexo
 5. 关联 Hexo 与 GitHub Pages
 6. GitHub Pages 地址解析到个人域名

我的博客：[一个拖延症患者的自我救赎](http://tlight.site/)

<!--more-->

------

### 1. 搭建 Node.js 环境

在 [Node.js 官网](https://nodejs.org/en/ ) 下载安装包 `v8.11.0 LTS`
(版本无特别要求，仅是编写博客时的最新版本）

保持默认配置，直接一路`Next`安装完成，若安装过程提示有加入环境变量的选项，可勾选。（否则应记下Node安装路径，以便于环境变量的添加，如果安装完成后输入后续命令时出现找不到的情况，请检查环境变量是否以添加，由于不难，为了节省篇幅这里就不累赘了）

使用`Win+R`组合键打开运行输入框，输入`cmd`打开命令提示符。
输入`node -v`和`npm -v`，出现版本号则说明 Node.js 环境配置成功，第一步完成！

### 2. 搭建 Git 环境

我们需要通过Git把本地的网页和文章等提交到 GitHub 上。

Git 是一款免费的、开源的、分布式的版本控制系统。旨在快速高效地处理无论规模大小的任何软件工程。

每一个 Git克隆 都是一个完整的文件库，含有全部历史记录和修订追踪能力，不依赖于网络连接或中心服务器。其最大特色就是“分支”及“合并”操作非常快速、简便。

在 [Git 官网](https://git-scm.com/) 下载安装包 `Git-2.16.3-64-bit.exe`，默认配置安装。

桌面右键，打开`Git Bash Here`,输入`git --version`（这里并没有-v的简写形式）出现版本号即说明Git环境搭建成功，第二步完成。

### 3. GitHub配置

GitHub 是一个代码托管平台，因为只支持 Git 作为唯一的版本库格式进行托管，故名 GitHub。

如果你想更加深入了解，可以查看[为什么要使用 GitHub](https://github.com/WD-iGaves/Mars.js/wiki/%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E4%BD%BF%E7%94%A8GITHUB%3F)

在[GitHub主页](https://github.com/)登入账号后，进行下列操作。

`start project`->`Repository name`填写`Owner.github.io`
(Owner就是你的username,由于我已经创建过了，所以有红字提示)
![ alt 创建Repository](http://p6bx8q1l3.bkt.clouddn.com//18-3-29/96111296.jpg)

访问 Owner.github.io，如果可以正常访问，那么 Github 的配置已经结束了。

前置相关环境配置已经完成，下面开始讲解 Hexo 的相关操作

### 4. 安装配置 Hexo

Hexo 是一个快速、简洁且高效的博客框架，使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。更多详情你可以参阅[Hexo 中文官网](https://hexo.io/zh-cn/)

由于已经安装了Node.js，可以使用自带的npm工具安装Hexo。
使用`Win+R`组合键打开运行输入框，输入`cmd`打开命令提示符。

    npm install hexo-cli -g

可能会出现WARN，但不会影响正常使用。

在`Git Bash`（也可在命令提示符中）中输入`hexo -v`查看Hexo版本，以确保安装成功。

安装 Hexo 完成后，请执行下列命令来初始化 Hexo，注意改成自己的用户名，Hexo 将会在指定文件夹（我将有关文件放在了blog）中新建所需要的文件。
第一步clone你刚才创建的GitHub Repository，我将它克隆到了`E:/Github`目录下。

```bash
	cd e:/Github
	git clone https://github.com/tuliang1996/tuliang1996.github.io.git
    cd e:/Github/tuliang1996.github.io/
    hexo init blog
    cd blog/
    npm install
```

`init` 命令会在当前目录下新建你所指定的文件夹并生成一系列文件，指定文件夹的目录如下：

    .
    ├── .deploy         #需要部署的文件
    ├── node_modules    #Hexo插件
    ├── public          #生成的静态网页文件
    ├── scaffolds       #模板
    ├── source      #博客正文和其他源文件，404、favicon、CNAME 都应该放在这里
    | ├── _drafts       #草稿
    | └── _posts        #文章
    ├── themes          #主题
    ├── _config.yml     #全局配置文件
    └── package.json    #npm 依赖等

运行本地 Hexo 服务

```bash
    hexo server
    或者
    hexo s
```

之后，您的网站会在[http://localhost:4000](http://localhost:4000)下启动。如果能够正常访问，则说明 Hexo 本地博客已经搭建起来了，下一步就是部署到Github。(当你对网站内容修改后可以先hexo s在本地查看效果后，如果达到自己期待的效果，再部署到Github网页上)

#### Notice

执行hexo server找不到指令
解决办法：打开`git bash`或者命令提示符安装`hexo-server`。

```bash
    sudo npm install hexo-server
    或者
    npm install hexo -server --save
```

### 5. 关联 Hexo 与 GitHub Pages

利用SSH keys让本地git项目与远程的GitHub建立联系。

#### 5.1 生成SSH keys

打开`git bash`，输入以下命令，需要输入你自己的邮箱

```bash
    ssh-keygen -t rsa -C "you email"
```

回车后会提示输入key文件保存地址，可选择默认（C:\Users\Username\.ssh\id_rsa.pub）。
再次回车提示输入密码，若私人电脑可以直接回车，不设置密码。（若设置了，每次提交时都需要输入该密码）

#### 5.2 添加 SSH Key 到 GitHub

打开C:\Users\Username\.ssh\id_rsa.pub，复制全部内容（即刚才生成的密匙），粘贴到 [GitHub SSH设置](https://github.com/settings/ssh) 的 new SSH key 中。

添加完成后，在`git bash`中输入以下命令：

```bash
    ssh -T git@github.com
```

如果是下面的反馈：

    The authenticity of host 'github.com (207.97.227.239)' can't be established.
    RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
    Are you sure you want to continue connecting (yes/no)?

输入yes即可，若你上一步中设置了密码，这一步中需要输入密码。你会得到反馈：

    Hi tuliang1996! You've successfully authenticated, but GitHub does not provide shell access.

#### 5.3 配置Git个人信息及本地Deployment

现在你已经可以通过 SSH 链接到 GitHub 了，仍需要完善一些个人信息，Git 会根据用户的名字和邮箱来记录提交，GitHub 也需要用到这些信息来做一些权限的处理。

```bash
    git config --global user.name "your name"
    git config --global user.email "your email"
```

然后打开本地博客存储根目录，打开`_config.yml`文件（你可以使用记事本），使用`ctrl + F`搜索`Deployment`，然后依照下面进行修改，请保留各属性冒号后面的空格。

    # Deployment
    ## Docs: https://hexo.io/docs/deployment.html
    deploy:
        type: git
        repo: git@github.com:tuliang1996/tuliang1996.github.io.git
        branch: master

这代表你将使用git来提交你的修改，repo属性即地址，提交修改的分支为master。

#### 5.4 提交修改到 GitHub Pages

```bash
    // 删除旧的 public 文件
    hexo clean

    // 生成新的 public 文件
    hexo generate
    或者
    hexo g

    //    开始部署
    hexo deploye
    或者
    hexo d
```

你可以简写为

```bash
    hexo d -g
```

在浏览器中输入你的站点（我的是`tuliang1996.github.io`），如果与你刚才在本地运行localhost看到的页面一样，那么恭喜你，Hexo 与 GitHub Pages 已经成功关联了。

你可能会注意到你的GitHub项目中并没有提交你blog文件夹下的所有文件，这是因为文件夹下的`gitignore.txt` 文件定义了在通过hexo命令提交时所忽略的文件，所以你的master分支上并不是所有文件。这样做是有利的，便于管理。但是当你在多端进行博客维护时可能会造成麻烦，这时你可能需要新建一个分支，用这个分支提交所有的文件，然后当你在另一台电脑上需要维护博客时，可以从这个分支拉取，而提交博客仍使用hexo指令。

关于`Git` 指令的操作，你可以看我自己搭建的个人博客[一个拖延症患者的自我救赎](http://tlight.site/) 中的[Git 基本指令](http://tlight.site/2018/04/03/Git-%E5%9F%BA%E6%9C%AC%E5%91%BD%E4%BB%A4/)这篇文章，它或许会对你产生一点帮助。

#### Notice2

1 Deploye操作失败

解决办法：安装`hexo-deployer-git`扩展

    npm install hexo-deployer-git --save

2 如果在执行`hexo d`后,出现`error deployer not found:github`的错误

可能是因为配置文件没有设置好，或者 public key 所致，重新详细设置即可。

3 .md文件因为hexo的执行而被解析成html，但GitHub通常建议附上README.md说明文件，如何保留README.md

Hexo原理就是hexo在执行hexo generate时会在本地先把博客生成的一套静态站点放到public文件夹中，在执行`hexo deploy`时将其复制到.deploy_git文件夹中。所以，在执行`hexo deploy`前把在本地写好的README.md文件复制到.deploy_git文件夹中，再去执行`hexo deploy`即可。

### 6 GitHub Pages 地址解析到个人域名

Github Pages 是面向用户、组织和项目开放的公共静态页面搭建托管服务，站点可以被免费托管在 Github 上（有300M的免费空间），你可以选择使用 Github Pages 默认提供的域名 github.io 或者自定义域名来发布站点。

如果你觉得这个二级域名不够个性，想拥有一个属于自己的域名，你可以通过以下方法来实现。（反正现在首年便宜，玩玩嘛）

首先你需要在阿里云或腾讯云购买一个域名，然后，在本地博客存储根目录下的`source`文件夹内创建名为`CNAME`的文件，你可以创建一个文本文档，然后改名为`CNAME`即可，注意同时删去它的后缀。用记事本或其他编辑器打开它，输入你的域名，不能加`http://`或`https://`。

以阿里云购买的为例，进入[域名解析控制台](https://dc.console.aliyun.com/dns/)，解析控制，添加解析
![alt 阿里云添加解析](http://p6bx8q1l3.bkt.clouddn.com//18-4-2/52401210.jpg)

 1. 记录类型选择CNAME
 2. 主机记录填www
 3. 解析线路选择默认
 4. 记录值填yourname.github.io
 5. TTL值为10分钟
 6. 再添加一个解析，记录类型A
 7. 主机记录填@
 8. 解析线路选择默认
 9. 记录值填你 GitHub 的ip地址（在命令提示符中`ping`：）

    ping tuliang1996.github.io

![alt 阿里云域名绑定](http://p6bx8q1l3.bkt.clouddn.com//18-4-2/81948236.jpg)

保存后，当你输入你所购买的域名（我的`tlight.site`)，或者GitHub Pages中的`github.io`域名（我的`tuliang1996.github.io`）均会跳转到`tlight.site`。相当于一个重定向的过程。（如果失败可以稍等几分钟之后重试）

### 结尾

以上就是较为详细的搭建过程了，参考过很多人的教程。
如：
https://www.jianshu.com/p/39562a0d8eb6?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation
以及
https://www.jianshu.com/p/0321cb243963
可能还有没有提到的那些文章的帮助。在此表示感谢。