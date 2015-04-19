---
layout: post
title:  Sublime Text 3使用总结
date:   2015-03-20
categories:
- linux
tags:
- sublime
---

Sublime Text 3使用了一段时间，确实好用，现在可以删除SourceInsight和UtraEdit了。<br />
记得谁说过一句话，“改变我人生观的是Vim，改变我世界观的是Emacs，改变我价值观的是Sublime Text”。<br />
虽然有点夸张，但这足以印证这些神×编辑器的强大。<br />
对Sublime Text的使用过程和技巧进行了总结，后续会持续更新。


##Sublime Text的优点
* 可管理各种类型的工程
* 文件加载与查找速度奇快
* 通过插件进行扩展

##Sublime Text的插件
* Package Control：插件管理
* Markdown To Clipboard：markdown转html的工具
* Markdown Editing：markdown编辑增强
* Ctags：代码跳转工具
* SublimeREPL：直接在编辑器中运行一个解释器，支持Python和Ruby
* ClickableURLs: 可以让文件中的URL可点击
* Soda Theme: 更换ST的外观主题
* SideBarEnhancements: 改进ST的侧边栏，

##Sublime Text使用技巧
* Ctrl+p  搜索所有Project中的文件
* Ctrl+r  搜索当前文件的函数
* Ctrl+Shift+r  搜索所有Project中的函数
* Ctrl+g+行号  快速跳转到指定的行
* Ctrl+M 光标跳至对应的括号
* Ctrl+L 选择整行（按住-继续选择下一行）
* Ctrl+Shift+K 删除整行
* Ctrl+D 选词 （按住-继续选择下个相同的字符串）
* Ctrl+/ 整行注释或取消（如已选择内容，同“Ctrl+Shift+/”效果）
* Ctrl+Shift+/ 已选择内容注释或取消
* Ctrl+Shift+[ 折叠代码
* Ctrl+Shift+] 展开代码
* Tab 自动完成缩进
* Shift+Tab 去除缩进
* Shift+右键   选择列模式文本

##使用Sublime Text遇到的问题
* 无法显示或输入中文：通过Package Control安装ConvertToUTF8或GBK Encoding Support
* windows下Ctags安装后不能使用：在[ctags官网](http://ctags.sourceforge.net/)下载最新版本，将解压缩后的ctags.exe放到系统环境变量的搜索路径中，一般是C:\windows\system32
