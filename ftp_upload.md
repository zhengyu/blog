---
author: zhengyu
title: linux使用命令行操作ftp
description: linux使用命令行操作ftp，linux ftp上传文件脚本
date: 2017-06-02 17:44:20
updated: 2017-06-02 17:44:24
comments: true
tags: 
    - linux
    - ftp
    - lftp
categories: 
    - linux
---

由于公司软件比较特殊，我们发布版本通常会给很多服务器都一并发布了，而且用的还是ftp，如果每个服务器都先打开ftp软件，连接上，然后再上传，这样无疑不符合我们程序员懒的特质。

于是我就花时间写了一个这样的脚本用来批量上传文件，我在这里会使用到lftp这个软件，这个软件比系统自带的ftp命令强大多了，而且ftp命令是不支持覆盖现有的目录的，所以还在苦苦寻找使用ftp覆盖目录的童鞋可以不用再继续了，下面就是简单介绍一下lftp的基本操作。

lftp的操作基本和ftp命令大同小异，首先打开ftp连接：`lftp username:password@ip`，打开链接后，你需要使用`cd`命令进入到你需要上传的目录，然后通过`lcd`命令进入到本地的目录，这时你就可以通过`put`、`mput`、`mirror`等命令上传文件，或通过`get`、`mget`等命令下载文件，最后可以通过`exit`退出ftp。

lftp主要命令

| 命令 | 作用 |
| --- | --- |
| help | 产看命令列表  |
| ls | 显示远端文件列表 |
| cd | 切换远端目录 |
| get | 下载远程文件（单文件）|
| mget | 下载远程文件（多文件）|
| pget | 使用多个线程来下载远端文件 |
| mirror | 同步目录，可以用于下载和上传(- R)目录 |
| put | 上传文件（单文件） |
| mput | 上传文件（多文件）|
| mv | 移动文件（可以重命名目录、文件）|
| rm | 删除远端文件 |
| mrm | 删除多个文件，可以使用通配符 |
| mkdir | 创建目录 |
| rmdir | 删除目录 |
| pwd | 显示远端的当前目录 |
| lcd | 切换本地目录 |
| lpwd | 显示本地目录 |
| exit | 退出ftp |

示例:

```bash
# 连接数据库
lftp username:password@127.0.0.1

# 列出当前ftp目录的文件
lftp username@127.0.0.1:~> ls

# 进入ftp上的某个目录
lftp username@127.0.0.1:~> cd testDir

# 定位到本地目录
lftp username@127.0.0.1:~> lcd /local/testDir

# 上传单个文件
lftp username@127.0.0.1:~> put testFile

# 上传多个文件，可以使用通配符
lftp username@127.0.0.1:~> mput *.md

# 将本地的目录同步到ftp上
lftp username@127.0.0.1:~> mirror -R . 

# 获取远程的文件
lftp username@127.0.0.1:~> get testFile

# 获取远程的多个文件
lftp username@127.0.0.1:~> mget testFile

# 将远程的目录同步到本地
lftp username@127.0.0.1:~> mirror . 

# 退出ftp
lftp username@127.0.0.1:~> exit
```

ftp上传脚本：
```bash
#!/bin/bash

pathStr=`pwd`

# 将脚本所在的目录的所有文件都上传到ftp上
# 参数为ftp连接信息 username:password@ip
lftp $1 <<EOF
lcd $pathStr
mirror -R -c .
rm ftp.sh
exit;
EOF
```

[lftp官网](http://lftp.yar.ru/)