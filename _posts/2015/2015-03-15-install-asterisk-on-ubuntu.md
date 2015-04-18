---
layout: post
title:  ubuntu下安装Asterisk
date:   2015-03-15
categories:
- linux
tags:
- open source, asterisk
---

最近在研究CLI，在[chinaunix](http://download.chinaunix.net/download.php?id=44893&ResourceID=566)上发现一个好玩的软件。
asterisk可以在linux环境下作为纯软件部署，提供VoIP PBX电信功能，基于开源代码进行扩展定制。
比较好奇，这款开源电信软件的架构设计与代码实现如何。


> Asterisk是一个开放源代码的软件VoIP PBX系统，它是一个运行在Linux环境下的纯软件实施方案。Asterisk是一种功能非常齐全的应用程序，提供了许多电信功能，能够把你的x86机 器变成你自己的交换机，还能够当作一台企业级的商用交换机。Asterisk让人激动的事情是它在小企业预算可承受的范围内提供了商业交换机的功能和可伸 缩性。你可以使用一台老式的奔腾3计算机，让你的机构看起来就同世界上的大企业一样。

###安装过程:
Step 1: 到[官网](http://www.asterisk.org/downloads)下最新的包

Step 2: 解压缩，进入安装包目录执行安装脚本检测环境和配置，可能会因为少包而出错
```sh
$ cd asterisk-11.0.1
$ ./configure
```
![asterisk_install](http://7wy3wu.com1.z0.glb.clouddn.com/asterisk-11.0.1-install-1.png)

Step 3: 出现星号图案表示安装成功，进行安装
```sh
$ make clean
$ make all
$ make install
```
![asterisk_install](http://7wy3wu.com1.z0.glb.clouddn.com/asterisk-11.0.1-install-2.png)

根据提示安装samples和progdocs

Step 4: 键入下面两个命令进入控制台
```sh
$ asterisk
$ asterisk -r
```

###遇到的问题：

* 缺少libxml2开发包
configure: \*** XML documentation will not be available because the ‘libxml2′ development package is missing.
configure: \*** Please run the ‘configure’ script with the ‘–disable-xmldoc’ parameter option
configure: \*** or install the ‘libxml2′ development package.
```sh
$ apt-get install libxml2-dev
```

* 缺少SQLite3开发包
Warning: Install SQLite3 development packege
```sh
$ apt-get install libsqlite3-dev
```

* 缺少ncurses开发包
configure: error: \*** termcap support not found (on modern systems, this typically means the ncurses development package is missing)
```sh
$ apt-get install ncurses-dev
```

> * ubuntu版本为14.04 LTS
> * asterisk版本为11.0.1
> * 安装过程参考了[这里](http://www.linuxidc.com/Linux/2013-01/78299.htm)

