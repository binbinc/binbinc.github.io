---
layout: post
title:  PaperWork源码阅读＃1：入门篇
date:   2016-10-12
categories:
- coding
tags:
- 源码阅读
---


下面是[GitHub][1]上对Paperwork的介绍，

    Paperwork aims to be an open-source, self-hosted alternative to services like Evernote ®, Microsoft OneNote ® or Google Keep ®.
    
    Paperwork is written in PHP, utilising the beautiful Laravel 4 framework. It provides a modern web UI, built on top of AngularJS & Bootstrap 3, as well as an open API for third party integration.
    
    For the back-end part a MySQL database stores everything. With such common requirements (Linux, Apache, MySQL, PHP), Paperwork will be able to run not only on dedicated servers, but also on small to mid-size NAS devices (Synology ®, QNAP ®, etc.).

PaperWork 采用 PHP 开发，前端采用了 Laravel 4 框架，基于 AngularJS 和 Bootstrap 3 构建，提供开放 API 用于第三方集成，后端基于 MySQL 数据库。

计划按照如下顺序学习PaperWork，Laravel 4 -> AngularJS ->  Bootstrap 3 -> MySQL。

只要熟悉Laravel 4就能在PaperWork上继续开发，内部代码都在frontend/app/目录下，切换到fronted目录，执行命令:

> npm install

将会通过package.json安装依赖包，这样运行gulp就可以运行自己的generator 进程了。


----------
相关的学习资源如下：

- [laravel学院][2]


  [1]: https://github.com/twostairs/paperwork
  [2]: http://laravelacademy.org