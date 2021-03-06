---
layout: post
title: Understanding Unix/Linux Programing Chapter 3 lesson 4
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

昨天加班到两点，回到家后磨蹭到3点多才躺下，今天实在是困得不行了，在眼皮快要睁不开之前强行让自己看了几页书。

### 今天的知识点：设置和修改文件的属性
- **一个普通文件的属性如下：**
```bash
-rwxr-xr-x   1 klin  staff   583B Sep  7 00:56 aboutme.md
```
- **文件类型：** 文件类型有普通文件、目录文件、设备文件、socket文件、符号链接文件、命名管道文件等等
    1. 文件类型的建立：文件类型是在文件创建的时候确定的，比如系统调用`create`可以创建一个普通的文件，其他文件类型有不同的系统调用，`是哪些系统调用呢？`
    2. 修改文件类型：很遗憾，这里做不到，文件类型一旦创建就无法改变
- **许可位和特殊属性：** 每个文件有9个许可位（权限位）和3个特殊属性位
    1. 建立文件的模式：`create`的第二个参数指定了文件的许可位，如：
    ```c
    fd = create("newfile", 0744);
    ```
    这个参数只是请求，而不是命令。内核真正创建文件的时候会根据"新建文件掩码"（file-creation-mask）来确定文件的最终模式。"新建文件掩码"是一个系统变量，它指定创建文件的时候哪些位需要被关掉。"新建文件掩码"可以通过`umask`来设置，如设置为022：
    ```c
    umask(022);
    ```
    就会在创建文件时自动关闭用户组和其他人对文件的写权限。
    2. 改变文件的模式: 具体参考`<sys/stat.h>`中定义
    程序可以通过系统调用`chmod`来改变文件的模式，如：
    ```c
    chmod("/tmp/myfile", 04764);
    chmod("/tmp/myfile", S_ISUID|S_IRWXU|S_IRGRP|S_IWGRP|S_IROTH);
    ```
    3. 修改文件许可权限和特殊属性的命令：chmod
- **文件的链接数：** 文件被引用的次数，增加文件的别名`link`会使链接数增加，减少别名`unlink`会使链接数减少。
- **文件的所有者和组：**
    1. 文件所有者：用户通过`create`创建文件时，文件的所有者为运行程序的用户，如果程序具有set-user-ID位，则新文件的所有者就是程序文件所有者。`passwd`命令修改用户密码是同样的道理，用户没有`/etc/passwd`文件的权限却可以修改自己的密码，就是因为`passwd`文件设置了`set-user-ID`位。
    2. 组：通常情况下，新文件的组就是执行创建动作的用户所在的组，在有些情况下会被设置与父目录的组相同，`那是什么时候呢？`
    3. 修改文件所有者和组：通过系统调用`chown`来修改文件的所有者和组：
    ```c
    chown("file1", 200, 40);
    ```
    将文件`file1`的所有者修改为uid=200的用户，组修改为gid=40的组。如果后面两个参数是-1，则不会变化。具体参考`<unistd.h>`
    4. 用命令修改所有者和组：参考`chown`和`chgrp`的联机帮助。
- **文件大小：** 文件、目录和命名管道的大小是它们实际占用的存储空间字节数目。会自动变化，不存在能直接减少文件大小的函数。`create`方法可以把文件的大小设置为0，相当于删除文件内容了吧。
- **时间：** 每个文件有三个时间：最后修改时间（modificationtime）、最后访问时间（access）和属性（如用户所有者ID、许可权限）最后修改时间，当文件被操作的时候，内核会自动修改这些时间，可以编程来修改最后修改时间和最后访问时间。
    1. 修改最后修改时间和最后访问时间：`utime`系统调用可以修改文件的最后修改时间和最后访问时间，具体参考`<utime.h>`。
    2. 用系统命令修改最后修改时间和最后访问时间：参考`touch`的联机帮助。
- **文件名：**
    1. 文件名的简历：`create`制定文件模式的同时可以指定文件名。
    2. 修改文件名：系统调用`rename`可以修改文件/目录的名字，还可以移动文件。具体参考`<stdio.h>`中`rename`函数使用方法。
    3. 系统命令修改文件名：`mv`
    
古耐。