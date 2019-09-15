---
layout: post
title: Understanding Unix/Linux Programing Chapter 4 lesson 1
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

中秋前后又荒废了几天时间，还好假日最后一天好好休息了一下，找回一点学习的感觉，看了这一章才发现自己与计算机专业出身的差距，之前都没有了解过inode相关的知识，看到这个名词都一脸懵逼，最近看了这一章才对Unix文件系统有了一个全局的了解，废话不多说，进入主题。

### 今天的知识点：Unix文件系统的内部结构
硬盘是一些磁性盘片组成的一个设备，书中应该是指的机械硬盘，固态硬盘应该又不一样了，前面提到的文件系统是对这个设备的多层次抽象。
- **第一层抽象：** 从磁盘到分区
- **第二层抽象：** 从磁盘到块序列, 前面这两部分都是硬件相关的知识，暂时留空
- **第三层抽象：** 这一层是Unix文件系统的核心，前面两层的抽象之后，磁盘被分成了有编号的磁盘块，Unix文件系统把这些磁盘快分成了三部分，如下图：
![unix_file_system](/img/unix_file_system.jpg){: .center-block :}


古耐。