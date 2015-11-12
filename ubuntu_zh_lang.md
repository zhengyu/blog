<!--
author: zhengyu
date: 2015-11-12 13:31:02
title: ubuntu添加中文支持
tags: ubuntu
category: linux
status: publish
summary: ubuntu 14.04 添加中文支持
-->

原本服务器的ubuntu系统只支持因为，由于一直也没有什么时间，所以也就没有管，最近实在是真的需要中文了，所以也就给系统装上了中文。

操作也不困难，具体操作如下：

1. 在命令行中输入：

```shell
sudo apt-get install language-pack-zh-hans language-pack-zh-hans-base
```

输入管理员密码后，就会自动下载中文的字库。

2. 配置相关环境变量:
编辑文件```/etc/environment```，添加以下设置：

```shell
LANG="zh_CN.UTF-8"
LANGUAGE="zh_CN:zh:en_US:en"
```

添加完成后需要加载配置，使用命令```sudo dpkg-reconfigure locales```，待配置加载完成后既可。



