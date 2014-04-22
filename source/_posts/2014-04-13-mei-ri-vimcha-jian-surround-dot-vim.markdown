---
layout: post
title: "每日vim插件--surround.vim"
date: 2014-04-13 22:11
comments: true
categories: [vim]
---
今天介绍一个必备的插件——[surround.vim](https://github.com/tpope/vim-surround),来自Tim Pope大神，很多著名的插件包括pathogen.vim都是出自他手，后面我还会介绍更多他写的插件。

是我最常用的插件之一。这个插件专门用来处理成对的包围符号……比如说括号，单双引号，XML标签等。

使用也非常简单好记，遵循vim本身的规则。

先来看一组实例，就知道这个插件的用途和使用方式了：

光标在

	"Hello world!"
	
中时按下`cs"'` ，则会替换双引号为单引号：

	'Hello world!'


	
<!--more-->
	
继续按下`cs'<q>`，则会替换单引号为<q>标签

	<q>Hello world!</q>

	
按下 `cst"`，则回到初始的双引号：

	 "Hello world!"

要删除符号，则按下`ds"`

    Hello world!
    
当光标在hello上时，按下`ysiw]`，则会变为

	 [Hello] world!
	 
这个操作为其加上了包围符号。

总结下：

1.删除包围符号的命令是`ds`,后面加的字符表示要删除的符号。比如：

	"Hello *world!"           ds"         Hello world!
	
2.替换包围符号的命令是`cs`,命令后跟两个参数，分别是被替换的符号和需要使用的符号。比如

	"Hello *world!"           cs"'        'Hello world!'
	
3.添加包围符号的命令是`ys`(ys可以记为you surround)，命令后同样跟两个参数，第一个是一个vim“动作”（motion）或者是一个文本对象。

其中motion即vim动作，比如说`w`向后一个单词。文本对象简单的来说主要是来通过一些分隔符来标识一段文本，比如`iw`就是一个文本对象，即光标下的单词。不理解的朋友可以将光标放置在单词hello的中央，分别试一下`ysw`和`ysiw`的区别应该就明白啦。如果大家需要详细介绍motion和文本对象，可以留言或者直接公众账号回复，我看看要不要单独介绍下。

	  Hello w*orld!             ysiw)       Hello (world)!
	  
另外：`yss`命令可以用于整行操作，忽略中间的空格。
`yS`和`ySS`还能让包围内容单独一行并且加上缩进。

4.添加包围符号还有个非常好用的方式：在可视模式v下，按下`S`后即可添加想要添加的包围符号了。

再说一个小技巧：在包围符号为括时，输入左括号`(或者{`,则会留一个空格

	Hello w*orld!             ysiw(       Hello ( world )!
	
而右括号则不留空格，也是非常好用，看编码风格使用。

今天就介绍到这里，欢迎关注我的公众账号，最新的文章都会第一时间推送到。有问题可以直接回复。
