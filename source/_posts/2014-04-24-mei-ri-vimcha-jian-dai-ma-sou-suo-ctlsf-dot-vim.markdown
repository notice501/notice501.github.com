---
layout: post
title: "每日vim插件--代码搜索ack.vim/ag/ctlsf.vim"
date: 2014-04-24 19:11
comments: true
categories: [vim]
---
#ack.vim

[ack.vim](https://github.com/petdance/ack)。应该是大多数vimer的必装插件。

在写这篇文章的时候才发现ack已经发布了2.0版本，并且ack 1已经不再维护。但是两者差别不大。这里介绍的基于2.0版本。

ack就是一个代码搜索工具，类似grep，用perl编写，充分利用了perl对正则的强大处理能力。为什么用ack而不是直接用grep？ack号称超越了grep。我基本认同。

1. ack会显示搜索到的行号列号。
2. 会自动忽略.git这样的文件类型。
3. 速度很快。
4. 最重要的是相对grep是一个文本搜索工具，ack就是一个代码搜索工具。你只会搜索到你的js文件，php文件，而不会搜到一些意外的文件类型。

<!--more-->

再也不用这样输入：

	grep foo $(find . -name '*.pm' | grep -v .svn)


ack的使用很简单，命令输入：

	ack [OPTION]... PATTERN [FILES OR DIRECTORIES]

如果不输入文件或者文件夹，则默认在当前目录及子目录下搜索。

ack大致有如下几类OPTION，

1. 搜索选项，例如-i, --ignore-case ，忽略pattern的大小写
2. 搜索结果处理选项，例如 -l，只打印有匹配的文件名。
3. 搜索输出展现选项，例如--heading，在头部输出匹配文件的文件名
4. 文件搜索，是的，他还是find。例如 `Ack -f servicemodel` 查找servicemodel相匹配的文件。
5. 文件过滤。例如 --[no]ignore-dir=name  从待搜索目录中添加或删除目录。

具体的option可以查看ack文档.

这些option都可以直接配置到.ackrc中，作为默认配置。全局的ackrc放置于`/etc/ackrc`,用户的放在`$HOME/.ackrc`,仅仅用于某项目的就放在项目根目录中。

ack搜索结果如图所示：

![](http://ww3.sinaimg.cn/large/69d56e38gw1efr1w9s9i8j21kw0ghafr.jpg)

会打开Quickfix窗口。显示文件名，对于的行列和该行内容。按`t`可以在新标签打开，按回车直接打开，按v分隔垂直窗口打开等等。和前天介绍的NERD Tree等大多数插件的操作类似。

# ag.vim

ack的用法就介绍到这里，这里还要介绍的是[ag](https://github.com/ggreer/the_silver_searcher).和ack没什么区别，只是更快。

OS X下安装：

	brew install the_silver_searcher

在vim中进行配置：

	let g:ackprg = 'ag --nogroup --nocolor --column'
	
也可以直接安装[ag.vim](https://github.com/rking/ag.vim).ack 的 Silver Searcher fork版本。

# ctrlsf.vim

作者原图：

![](https://camo.githubusercontent.com/fae368edf534ce2228eda41418cb55ee68e19c20/687474703a2f2f692e696d6775722e636f6d2f6d6c576a336d7a2e676966)

在安装了ack或者ag的基础上再安装该插件即可。正如它的名字，它提供了和sublime text 2中Ctrl-Shift-F 一样的搜索效果。和ack或者ag不同的是，不再是显示一行，而是显示整个上下文。非常好用。

除此之外，可以按下`p`进行预览，运行`:CtrlSFOpen`重新打开搜索结果窗口(默认选择后关闭搜索窗口)。这个插件也是来自国内的朋友。

今天就介绍到这。

