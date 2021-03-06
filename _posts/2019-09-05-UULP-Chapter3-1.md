---
layout: post
title: Understanding Unix/Linux Programing Chapter 3 lesson 1
subtitle: 学习日记
gh-repo: l33klin/understanding-unix-linux-programming
gh-badge:
  - star
  - fork
  - follow
tags:
  - 学习日记
  - UULP
comments: true
published: true
---

第一篇学习日记，《Understanding Unix/Linux Programing》这本书没有找到合适的缩略名，我就擅自主张地简写为UULP了，其实也没有认真查，哈哈哈就这样看吧。

今天看到的是第三章ls命令的实现，这让我想起了去年面试微信的一道题目：`删除一个文件夹下面的一个文件需要找个文件的什么权限和文件夹的什么权限？`恨我自己没有提前看到这本书哇，如果早看了这本书不是随随便便回答的么，哎，偏偏要踩一回坑才长记性。

### 今天学到的点：
- **目录是什么**：目录是一个特殊的文件，它的内容是文件和目录的名字，和前面一章的utmp文件类似。他们都包括很多记录，每个记录的格式由统一的标准定义。每条记录的内容代表一个文件或目录。
- **目录和普通文件的区别**是目录永远不会空，它至少包含两条记录：`.`和`..`，`.`代表当前目录，`..`代表父目录，在根目录`/`中，`..`也代表当前目录。
- **如何操作文件**：
    - 当然可以以读写文件的read、open、close来操作文件，在FreeBSD的系统中甚至可以用cat命令查看目录的内容，但是Unix支持不同的目录类型，如果使用操作文件的方法操作目录的会需要处理不同类型目录的结构细节，因此肯定不是最好的方法
    - Unix操作系统提供了一系列操作目录的系统调用，头文件dirent.h就包含了这些方法的定义，可以使用`man 3 readdir`来查看具体的使用方法
