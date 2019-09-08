---
layout: post
title: Understanding Unix/Linux Programing Chapter 3 lesson 2
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

今天回来稍微晚了一点，在家里忙活了一下再打两把游戏就十二点多了，洗完澡来到电脑前都一点多了，还是要多休息呀，要不然一天的工作效率都很低。

### 今天的知识点：
- **获取文件信息的方法：** 在目录文件中的记录中没有文件信息，[那么文件信息保存在哪？](https://stackoverflow.com/questions/57844005/where-file-status-info-actually-store)需要怎么读到呢？
    - 获取文件信息的方法：可以在<sys/stat.h>中查看，MacOS中使用`man 2 stat`可以查到帮助文档。<sys/stat.h>的文档可以查看[这里](http://man7.org/linux/man-pages/man2/stat.2.html)，要注意在不同系统上会有一些差别，在mac帮助文档查看到的又大不一样。
    - 
- **stat数据结构：**
```c
struct stat {
               dev_t     st_dev;         /* ID of device containing file */
               ino_t     st_ino;         /* Inode number */
               mode_t    st_mode;        /* File type and mode */
               nlink_t   st_nlink;       /* Number of hard links */
               uid_t     st_uid;         /* User ID of owner */
               gid_t     st_gid;         /* Group ID of owner */
               dev_t     st_rdev;        /* Device ID (if special file) */
               off_t     st_size;        /* Total size, in bytes */
               blksize_t st_blksize;     /* Block size for filesystem I/O */
               blkcnt_t  st_blocks;      /* Number of 512B blocks allocated */

               /* Since Linux 2.6, the kernel supports nanosecond
                  precision for the following timestamp fields.
                  For the details before Linux 2.6, see NOTES. */

               struct timespec st_atim;  /* Time of last access */
               struct timespec st_mtim;  /* Time of last modification */
               struct timespec st_ctim;  /* Time of last status change */

           #define st_atime st_atim.tv_sec      /* Backward compatibility */
           #define st_mtime st_mtim.tv_sec
           #define st_ctime st_ctim.tv_sec
           };
```
- **stat数据的使用**
    - st_mode是一个16位的二进制数，前四位代表文件类型，中间三位各自单独意义，最后九位分别代表所有者、组成员、其他人对文件的访问权限。
    ![st_mode](/img/st_mode.jpg){: .center-block :}
    - <sys/stat.h>中定义了很多宏，提供了对st_mode的操作使对st_mode的读非常简单，具体参考stat.h的说明
- **将uid和gid转换成用户名和组名**
    - 通常情况下我们认为所有的用户名和组信息都放在了`/etc/passwd`文件中，其实不然，对于很多网络计算机，这种方法是不起作用的。在这种系统中，用户信息是保存在NIS中的，所有的主机通过NIS来进行用户身份验证。本地只保存所有用户的一个子集以备离线操作。这种情况我们要怎么获取用户信息呢？
    - 不用慌，库函数`getpwuid`帮我们做了这件事情，它会先访问`/etc/passwd`查找用户信息，如果找不到会从NIS中获取用户信息。`getpwuid`和`struct passwd`的定义在`pwd.h`中。参考[这里]()，或者使用`man 3 getpwuid`获得说明文档。
    - 注意：如果用户创建文件后被删除了，我们通过`getpwuid`会获取到一个空指针。

身体撑不住了，睡觉。
