---
layout: post
title:  《数学之美》读书笔记
date:   2015-03-10
categories:
- reading
tags:
- math
---

数学是一门古老的学科，在人类历史发展和社会生活中发挥着不可替代的作用。
我们从计数/乘法表开始接触数学，用代数解X元方程，用立体几何证明三点共线，用微积分求曲线面积，用概率论计算第三次取到白球的概率，用运筹学找建超市的合理位置，用离散数学设计最短路径算法。
虽然离开学校后不常用非奇异矩阵和拉格朗日中值定理，但我们在生活中经常使用数学，比较买一送一和八折的优惠，计算银行利息单利和复利的差异。
可是如何用数学来解决科技工作中的实际问题，比如设计拼音输入法和解决地图导航问题，我们很少能有机会体会，毕竟解决这些问题才能充分体现出数学的优势和威力。
《数学之美》尽量用简单的语言阐述这些复杂问题背后蕴含的数学原理，和人类历史发展与数学的关系。
当然由于作者的背景，也对统计科学发展和自然语言处理的相关历史进行了翔实的介绍。
我想，如果未来有人能推出金融和制造业等其他行业的《数学之美》版本，应该也会很精彩。


## 文字和数字的产生

文字的出现是信息爆炸导致人的头脑装不下太多信息，数字的出现则是由于人的财产多到需要计数才能搞清楚

为什么使用十进制，因为人有十个指头

使用二十进制和19*19乘法表可能是玛雅文明发展非常缓慢的原因之一

罗马数字，I代表个，V代表5，X代表10，L代表50，C代表100，D代表500，M代表1000，解码规则是加减法，小数字出现在大数字左边为减，右边为加

如罗马数字Ⅰ，Ⅱ，Ⅲ，Ⅳ（IIII），Ⅴ，Ⅵ，Ⅶ，Ⅷ，Ⅸ，Ⅹ，Ⅺ，Ⅻ……，对应的阿拉伯数字是1，2，3，4，5，6，7，8，9，10，11，12

近代才出现在一个数字的上面画一横表示扩大1000倍

文字和语言背后的数学，常用字笔画少，符合信息论中最短编码原理

纸张发明以前，古文简洁难懂，同时期的口语却和今天的白话差别不大，是由于写字不容易要刻在竹简上，信息论中可以解释为窄信道上传递信息需要压缩


## 自然语言处理&人工智能--从规则到统计

图灵测试，让任何机器交流，如果人无法判断自己交流的对象是人还是机器，就说明这个机器有智能

基于规则的自然语言处理的缺陷，语法规则不全面，复杂的上下文相关语法导致计算机解析很困难，同时词的多义性导致语义的处理需要依靠常识

机器翻译和语音识别，不是靠计算机理解了自然语言而完成的，而是靠数学统计

基于统计方法的核心模型是通信系统加隐含马尔科夫模型


## 信息的度量

信息熵是对一个信息系统不确定性的度量

汉语的冗余度相对较小

不要把所有的鸡蛋放在一个篮子里，是最大熵原理的朴素说法

对一个随事件的概率分布进行预测，应当满足全部已知的条件，不要对未知的情况做任何假设


## 简单之美

术是做事方法，道是做事的原理和原则

布尔代数将逻辑和数学合二为一

香农使用布尔代数实现开关电路，使得布尔代数成为数字电路的基础

简单哲学，先解决80%的问题，再解决剩下的20%

简单方案稳定可靠--AK47


## 搜索相关的技术

PageRank，民主表决式网页排名技术，被链接数和高排名的网站贡献的链接权重

使用有限状态机进行地址分析

全球导航的关键算法是图论中的动态规划算法，将全程最短路线分解为一个个寻找局部最短路线的小问题

MapReduce，并行计算

信息指纹，对信息熵进行无损压缩编码产生一个随机数，识别垃圾邮件和视频盗版源

余弦定理， 把新闻变成一组数字，再用算法比较两组新闻的相似性，进行新闻分类


##书中提到的书

从1到无穷大

时间简史
