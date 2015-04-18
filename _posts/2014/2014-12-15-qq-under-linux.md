---
layout: post
title:  腾讯为什么不支持linux QQ
date:   2014-12-15 
categories:
- linux
tags:
- QQ
---

> * ubuntu版本为14.04 LTS

> * 详细的安装和使用方法，可以参考[这里](http://server.zol.com.cn/269/2696247_all.html)：


今天有点无聊，想看下linux下QQ能不能用

体验了一把，发现只有WebQQ能用，tar.gz和DEB包根本不可用

看来企鹅对linux下的QQ毫不在乎，都无法安装

[知乎](http://www.zhihu.com/question/19756827)上说，MAC和linux版本支持的不好是因为体量太小，想想也是，这些三高人士和geek谁会掏钱去买齐赤橙黄绿青蓝紫钻呢

### 使用WebQQ

通过http://w.qq.com/  使用WebQQ，可以使用基本功能



###  使用tar.gz包

![qq_tar_gz](http://7wy3wu.com1.z0.glb.clouddn.com/qq_under_linux_1.png)

解压缩后，点QQ没反应，难道是RP问题？



###  安装DEB包

安装过程中出现"The package is of bad quality"的问题，提示是beta版本同时没有联系方式，直接ignore

![qq_install_issue](http://7wy3wu.com1.z0.glb.clouddn.com/qq_under_linux_2.png)

解压缩安装过程中失败，显示version number not start with digit，不是数字开头

![qq_install_issue](http://7wy3wu.com1.z0.glb.clouddn.com/qq_under_linux_3.png)
