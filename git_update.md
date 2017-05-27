---
author: zhengyu
title: centos升级git
description: 将centos默认安装的Git 1.7.1升级到Git 2.9.4
date: 2017-05-27 11:30:57
updated: 2017-05-27 11:30:57
comments: true
tags: 
    - centos
    - git
categories: 
    - linux
    - git
---

安装需求的类库
======
```bash
sudo yum install perl-ExtUtils-MakeMaker package
sudo yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel asciidoc

# 下载libiconv
wget http://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.15.tar.gz
tar zxvf libiconv-1.15.tar.gz
cd libiconv-1.15
./configure --prefix=/usr/local/libiconv
sudo make
sudo make install
```

卸载centos默认的git
======
```bash
yum remove git
```

编译安装git
======
```bash
wget https://www.kernel.org/pub/software/scm/git/git-2.9.4.tar.gz
tar zxvf git-2.9.4.tar.gz
cd git-2.9.4
make configure
./configure --prefix=/usr/local/git --with-iconv=/usr/local/libiconv
sudo make
sudo make install

# 安装完成后需要给git做一个软连接，不然bash会报找不到命令的错误
sudo ln -s /usr/local/git/bin/git /usr/bin

# 然后打印一下版本号，如果有显示则安装成功了
git --version
```

报错修改
=======

* make 时出现 [perl.mak] Error 2
* 解决方法：yum install perl-ExtUtils-MakeMaker package

----

* git fatal: Unable to find remote helper for 'https'
* 解决方法：安装curl的相关类库：yum install curl-devel，然后重新编译安装git
运行configure时添加curl的相关参数--prefix=/usr/include/curl(这个路径可以通过whereis curl得到)



