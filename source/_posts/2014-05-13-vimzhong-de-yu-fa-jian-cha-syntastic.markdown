---
layout: post
title: "vim中的语法检查-syntastic"
date: 2014-05-13 19:29
comments: true
categories: 
---
最近比较忙，有几天没更新了。今天有个同事问我一个语法插件的问题，向他介绍了Sytanstic.vim。那今天就来介绍下这个必备的插件吧。

很多人喜欢IDE就是因为他的语法检查，有了[Sytanstic.vim](https://github.com/scrooloose/syntastic),这个问题就不复存在了。（当然，仅仅是语法检查）

##功能

上一张官方图：

![](http://foocoder.qiniudn.com/blog/syntasticsyntastic.png?token=hYfsyKwhHPe-Ga-1Hypx5F8CwimEywvTI8XdNpEm:z6zQYbdezgOYcKfzok7LEkuRDkg=:eyJTIjoiZm9vY29kZXIucWluaXVkbi5jb20vYmxvZy9zeW50YXN0aWNzeW50YXN0aWMucG5nIiwiRSI6MTQwMDA2ODU2N30=)

<!--more-->

图片很清楚的介绍了插件功能：

1. 用location list 列出所有错误。
2. 命令行窗口显示当前错误。
3. 错误标记，有警告和错误。
4. 鼠标悬停可以出现错误提示框
5. 状态栏标记。

##安装
Sytanstic目前支持了大多数常用语言，但是并不都打包在插件当中，而是可以按需安装相关的语法检查工具。在[wiki](https://github.com/scrooloose/syntastic/wiki/Syntax-Checkers)页面可以看到所有的语法检查包。

以javascript为例。在安装完syntastic之后（推荐使用vundle），我选择使用jshint来作为js的语法检查工具。

首先安装：

	npm install jshint -g
	
注意安装后jshint需要在系统环境变量中，否则需要指定path：

	g:syntastic_jshint_exec
	
指定jshint的配置文件目录：

	g:syntastic_javascript_jshint_conf
	
jshint的配置这里就不说了，参看[文档](http://jshint.com/docs/)

这样js语法检查就可以工作了。

##配置

当然也可以做一些简单的配置，比如设置为每次打开buffer就执行语法检查，而不只是在保存时：

	let g:syntastic_check_on_open = 1
	    
如果想使用多个检查器，可以这样写：

	let g:syntastic_php_checkers = ['php', 'phpcs', 'phpmd']

##错误跳转
syntastic使用location list来显示所有的错误，location list和quificfix 类似，包含了位置信息。

调起这个location list
	
	:Errors 或者 :lopen

使用`:lne[xt]`和`:lp[revious]`就可以在错误间跳转。当然，如果用的多，可以做个mapping。

今天就介绍到这里。