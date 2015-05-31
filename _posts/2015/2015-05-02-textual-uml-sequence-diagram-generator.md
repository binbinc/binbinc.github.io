---
layout: post
title:  用可描述性语言生成顺序图
date:   2015-05-02
categories:
- tool
tags:
- textual language, sequence diagram
---

大部分UML工具是通过鼠标拖拉的方式生成顺序图，但是对于大规模复杂系统，有大量顺序图维护会很不方便。<br />
通过可描述性语言(textual language) 生成顺序图，可以用维护代码的方式更新UML图，避免格式调整的时间，集中精力设计顺序图本身。<br />
对于使用textual language生成顺序图的方案，有几种解决办法：

> * 应用程序：ESSD(EventStudio System Designer)是最好用的付费工具，没有之一。如果是大公司壕，果断上ESSD付费版本。
> * PlantUml：第三方的开源可描述性UML语法标准，已经有很多现成的工具供参考。
> * Markdown+js：使用轻量级语法描述，通过js生成顺序图，有几个web应用可以使用。


###应用程序
[EventStudio System Designer 6](http://www.eventhelix.com/EventStudio/)<br />

###PlantUml
[PlantUML-Editor](http://www.codeproject.com/Articles/64328/PlantUML-Editor-A-Fast-and-Simple-UML-Editor-using)<br />

http://en.wikipedia.org/wiki/PlantUML<br />
http://www.plantuml.com/plantuml/form<br />
http://lib.custis.ru/images/c/c4/PlantUML_Language_Reference_Guide.pdf<br />
https://github.com/vbauer/lein-plantuml<br />
https://github.com/dougn/python-plantuml/<br />

###Markdown+js
[websequencediagrams](https://www.websequencediagrams.com/)<br />
[lucidchart](https://www.lucidchart.com/)<br />

http://bramp.github.io/js-sequence-diagrams/

