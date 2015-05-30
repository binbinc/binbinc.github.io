---
layout: post
title:  用LaTex制作个人简历
date:   2015-05-30
categories:
- tool
tags:
- LaTex, cv
---


妹子最近想换工作，需要用LaTex制作简历。<br />
想当年刚毕业时用word撸的简历，惨不忍睹。<br />
用LaTex刷简历，最好用现成的模板，不然从0开始就是找虐。<br />
过程整理如下，方便以后查阅。

###安装CTex

1. [下载](http://www.ctex.org/CTeXDownload/)CTex。
2. 如果安装中文版本，使用过程中出现问题提示对话框可能出现乱码，建议安装英文版本。
3. MiKTeX出现“connect failed in tcp_connect”问题。请检查主机的网络环境，是否打开了防火墙或是在公司内网需要设置代理服务器。在
“MiKTeX Package Manager(Admin)”-->“Repository”-->“Chang Package Repository”位置下，设置代理服务器（可选）和更换源地址。详细的解决过程请[参考](http://tex.stackexchange.com/questions/167562/miktex-connect-failed-in-tcp-connect)。
4. 在“MiKTeX Package Manager(Admin)”中查找到指定的package进行安装时出现“Windows API error 87: The parameter is incorrect ”问题。通常是已经开了一个MikTeX实例，阻塞了需要安装的package，先关掉这个MikTeX实例就可以解决问题。详细的解决过程请[参考](http://sourceforge.net/p/miktex/mailman/message/28809258/)。

###下载LaTex模板

有很多Latex模板供选择，挑选自己喜欢的模板：<br />
1. 标准模板[moderncv](http://www.ctan.org/tex-archive/macros/latex/contrib/moderncv/)<br />
2. 豆瓣上推荐的[rpi](http://rpi.edu/dept/arc/training/latex/resumes/)模板<br />
3. [latextemplates](http://www.latextemplates.com/cat/curricula-vitae)的模板

###通过LaTex模板moderncv生成CV

1. 在cmd模式下进入moderncv的目录，通过命令<code>pdflatex template-zh.tex</code>编译生成PDF。
2. 编译过程中如果出现“*.sty”找不到的问题，先退出编译过程，在“MiKTeX Package Manager(Admin)中安装好后回来继续编。
详细的解决过程请参考[1](http://www.zhihu.com/question/30102699?sort=created)和[2](http://tex.stackexchange.com/questions/25564/missing-file-from-the-package-symbol)。


###通过在线LaTex网站生成PDF

1. [sharelatex](https://www.sharelatex.com)
2. [overleaf](https://www.overleaf.com)
