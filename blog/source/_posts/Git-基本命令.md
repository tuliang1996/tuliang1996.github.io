---
title: Git 基本命令
date: 2018-04-03 10:01:34
tags:
---

------

本文旨在记录一下一些Git的入门指令。

Git 是一款免费的、开源的、分布式的版本控制系统。旨在快速高效地处理无论规模大小的任何软件工程。

每一个 Git克隆 都是一个完整的文件库，含有全部历史记录和修订追踪能力，不依赖于网络连接或中心服务器。其最大特色就是“分支”及“合并”操作非常快速、简便。

[为什么要使用 GitHub](https://github.com/WD-iGaves/Mars.js/wiki/%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E4%BD%BF%E7%94%A8GITHUB%3F)

<!-- more -->

------

### 获取帮助

```bash
    git help
```

### 初始化一个仓库，或者克隆现有的仓库

```bash
    git init
    git clone https://github.com/repository
```

`git init`会初始化当前文件夹为git仓库，并生成`.git`目录。
如果你是初始化一个本地仓库且需要与远程仓库建立联系，那么你需要先创建一个repository（这一步在github网页上完成）。
`git clone`会将url所在的仓库克隆到当前文件夹下。你也可以在url后面指定存放路径。

### 状态与暂存

一般来说，文件有两种状态

- 跟踪
- 未跟踪

我们可以输入命令

```bash
    git status
```

来查看当前仓库文件的状态，它会反馈出你当前分支的状态，以及暂存区和为跟踪文件的状态。

如果你仅仅克隆下来没做任何修改

    nothing to commit, working directory clean

如果你创建了新的文件或修改了文件，那么该文件会变成未跟踪状态

    Untracked files:
      (use "git add <file>..." to include in what will be committed)

        Newfile

    nothing added to commit but untracked files present (use "git add" to track)

我们可以把该文件添加到暂存区，使其变为跟踪状态，等待提交，通常可使用第二条指令，将当前文件夹内的所有文件全部存到暂存区。

```bash
    git add Newfile
    git add .
```

再次输入`git status`你会得到如下反馈

    Changes to be committed:
      (use "git rm --cached <file>..." to unstage)

        new file:   Newfile

还有一种简便的方式是`git status -s`，将反馈简短的状态信息，其中`A`代表新增到暂存区还未提交的文件，`??`代表未跟踪的文件，`D`代表删除了的文件。

### 提交与推送

当你完成修改，并将所有修改提交到暂存区后，你可以提交到本地仓库。

```bash
    git commit -m "commit message"
```

`-m`为必要选项，附带上你修改文件后所要提交的说明信息。

接下来你需要把你的提交推送到远程仓库。

要查看当前配置有哪些远程仓库，可以用`git remote`命令，它会列出每个远程库的简短名字。在克隆完某个项目后，至少可以看到一个名为origin 的远程库，Git 默认使用这个名字来标识你所克隆的原始仓库。

如果你是初始化了一个本地仓库，想要推送到远程仓库的话，你可以使用

```bash
    git remote add origin <address>
```

与远程仓库建立联系， `<address>`的格式为

    git@github.com:username/repository.git

之后你需要先拉取远程仓库的更新

```bash
    git pull origin master --allow-unrelated-histories
```

然后推送你的更新

```bash
    git push -u origin master
```

### 结尾

基本的操作指令就是这些了，关于一些进阶的指令，请等待后续文章的更新。

谢谢浏览~~
