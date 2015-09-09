---
layout: post
title:  linux下使用Terminator
date:   2015-09-09
categories:
- linux
tags:
- terminator
---

Ubuntu自带的终端是gnome-terminal，虽然能用但是不能支持屏幕分割和选择复制等功能，于是换用terminator作为默认终端。

**Ubuntu下这样安装terminator**

    sudo apt-get install terminator

**设置默认Terminal为Terminator**           

    gsettings set org.gnome.desktop.default-applications.terminal exec   /usr/bin/terminator
    gsettings set org.gnome.desktop.default-applications.terminal exec-arg "-x"

**修改配色方案**
在终端中打开右键菜单，配置文件->配置文件首选项->切换到颜色选项卡，
把使用系统主题中的颜色选项的勾去掉，
把文字颜色设为#708284，背景颜色设为#07242E，效果还可以。

**常用快捷键**
Ctrl+Alt+T   打开终端
Ctrl+l 相当于clear，即清屏
Shift+Ctrl+T 打开新的标签页
Shift+Ctrl+W 关闭标签页
Shift+Ctrl+C 复制
Shift+Ctrl+V 粘贴
Shift+Ctrl+N 打开新的终端窗口
Shift+Ctrl+o 上下拆分屏幕
Shift+Ctrl+e 左右拆分屏幕
Shift+Ctrl+w 关闭当前窗口
Shift+Ctrl+q 关闭整个终端
F11 全屏切换

