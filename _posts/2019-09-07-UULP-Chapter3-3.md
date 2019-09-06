---
layout: post
title: Understanding Unix/Linux Programing Chapter 3 lesson 3
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

吃完火锅回到家都11点了，遛完狗再洗完澡又到一点了，强忍着睡意再看一会

### 今天的知识点：
- **获取文件组信息：** 昨天介绍了通过`getpwuid`获取用户信息，那么组信息怎么获取呢？
    - 类似用户信息，组信息记录则保存在`/etc/group`文件中，组信息的第一个字段是组名，第二个字段是密码，第三个字段是组ID（GID），第四个是组中的成员，示例如下：
    ```bash
    root::0:root
    other::1:
    bin::2:root,bin,dameon
    sys::3:root,bin,sys,adm
    ```
    - 一个用户可以同时属于多个组，文件信息中列出来的是用户的主组（primary group）。那怎么区分哪个组是用户的主组呢？
    - 如同用户信息，在网络计算机中，组信息也可能是保存在NIS上的，这时候我们也可以通过`getgrgid`来获取用户组信息，使用方法可以参考[这里](http://man7.org/linux/man-pages/man0/grp.h.0p.html)，也可以使用`man 3 getgrgid`获取帮助。
- **三个特殊的位：** stat结构中st_mode成员前4位是标志文件类型的，后9位是记录权限的位，中间三位则是特殊属性标志位。
    1. set-user-ID位
    2. set-group-ID位
    3. sticky位
    
太晚了，脑袋有点迷糊了，这三个的概念明天再细说，古耐。