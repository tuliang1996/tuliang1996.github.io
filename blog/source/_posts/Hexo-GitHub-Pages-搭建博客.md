---
title: Hexo + GitHub Pages 搭建博客
date: 2018-03-29 10:56:09
tags:
---

------

大概流程：

	[TOC]

个人博客地址：[一个拖延症患者的自我救赎](http://tlight.site)

------
### 1. 搭建 Node.js 环境

> Hexo 博客系统是基于 Node.js 编写的

Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境，可以在非浏览器环境下，解释运行 JS 代码。

在 [Node.js 官网](https://nodejs.org/en/ ) 下载安装包 `v8.11.0 LTS`
(版本无特别要求，仅是编写博客时的最新版本）

保持默认设置，安装完成，若安装过程提示有加入环境变量的选项，可勾选。（否则应记下Node安装路径，以便于环境变量的添加）

使用`Win+R`组合键打开运行输入框，输入`cmd`打开命令提示符。
输入`node -v`和`npm -v`，出现版本号则说明 Node.js 环境配置成功，第一步完成！

![](http://p6bx8q1l3.bkt.clouddn.com//18-3-29/53183447.jpg)

### 2. 搭建 Git 环境

> 通过Git把本地的网页和文章等提交到 GitHub 上。

Git 是一款免费、开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。

在 [Git 官网](https://git-scm.com/) 下载安装包 `Git-2.16.3-64-bit.exe`，默认配置安装。

桌面右键，打开` Git Bash Here`,输入`git --version`出现版本号即说明Git环境搭建成功，第二步完成。

![](http://p6bx8q1l3.bkt.clouddn.com//18-3-29/92230219.jpg)

### 3. GitHub配置

GitHub 是一个代码托管平台，因为只支持 Git 作为唯一的版本库格式进行托管，故名 GitHub。
`start project`->`Repository name`填写`Owner.github.io`
(Owner就是你的username,由于我已经创建过了，所以有红字提示)
![](http://p6bx8q1l3.bkt.clouddn.com//18-3-29/96111296.jpg)

访问 Owner.github.io，如果可以正常访问，那么 Github 的配置已经结束了。

前置相关环境配置已经完成，下面开始讲解 Hexo 的相关操作

### 4. 安装配置 Hexo

> [Hexi 官网](https://hexo.io/zh-cn/)

Hexo 是一个快速、简洁且高效的博客框架，使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

由于已经安装了Node.js且其自带npm安装工具，使用npm安装Hexo。
使用`Win+R`组合键打开运行输入框，输入`cmd`打开命令提示符。

    npm install hexo-cli -g
    
可能会出现WARN，但不会影响正常使用。

在`Git Bash`（也可在命令提示符中）中输入`hexo -v`查看Hexo版本，以确保安装成功。

![](http://p6bx8q1l3.bkt.clouddn.com//18-3-29/63798959.jpg)

安装 Hexo 完成后，请执行下列命令来初始化 Hexo，注意改成自己的用户名，Hexo 将会在指定文件夹（我将有关文件放在了blog）中新建所需要的文件。、
第一步clone你刚才创建的GitHub Repository。

    cd e:/Github/tuliang1996.github.io/
    hexo init blog
    cd blog/
    npm install
    
输入以上命令后，指定文件夹的目录如下：

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

    hexo server
    或者
    hexo s
    
您的网站会在 http://localhost:4000 下启动。如果 http://localhost:4000 能够正常访问，则说明 Hexo 本地博客已经搭建起来了，下一步部署到Github。