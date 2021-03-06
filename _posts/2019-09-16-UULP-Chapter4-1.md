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
    - 超级块：文件系统的第一块被称之为超级块。这个块存放文件系统本身的结构信息。例如，超级块记录了每个区域的大小。超级块也存放违背使用的磁盘块的信息。Linux操作系统中可以使用以下命令查看超级块信息：
    ```bash
    # dumpe2fs -h /dev/sda1  # 这里认为/dev/sda1为根目录挂载的磁盘
    ```
    MacOS暂时没找到查看超级块信息的方法，也有可能MacOS上使用了其他的文件系统。注：macOS的文件系统，[参考](https://en.wikipedia.org/wiki/Apple_File_System)。
    - inode表：每个文件都有一些属性，如大小、文件所有者和最近修改时间等，这些性质被记录在一个称为inode的结构中。所有的inode信息都有相同的大小，inode表就是这些结构的一个列表。`怎么查看inode信息呢？`
    inode表中的每一个inode都通过位置来标识。如标识为2的inode就是inode表中第三个位置的inode。`系统是如何通过inode标识来找到真实的inode存放的位置呢？`
    - 数据区：文件的内容保存在这个区域，磁盘的每个块大小是一样的。如果文件内容超过了一个块的大小，则会保存在多个磁盘块中，`那么一个大文件很容易就保存在上千个磁盘块中，系统是如何追踪这些磁盘块的呢？`

一个问题：`文件系统创建的时候是如何确定inode表大小和数据区大小的呢？`
古耐。