---
layout: post
title: "vim 重复操作的利器--点命令"
date: 2014-05-06 20:18
comments: true
categories: 
---
![](http://ww4.sinaimg.cn/large/69d56e38gw1eg4vqmtvzsj20b408cdgf.jpg)

之前介绍过可以重复motion的插件space.vim，有朋友留言说`.`不是也可以？其实`.`确实可以重复很多动作，但是无法重复motion。其实`;`和`,`倒是可以重复motion。不过space.vim可以重复更多操作，之前的博客有全部列出来。

今天主要就介绍`.`命令。

这个命令就用来重复上一次的操作。比如:`dw`,再按`.`就会再删除一个单词。他可以重复的命令非常多，比如插入操作`a`,`i`，比如替换删除操作`c`,`s`, `r`还有`J`,`~`等常用的操作。但是我却很难概括他到底可以重复哪些操作，我基本归纳为重复对当前buffer造成改变的操作，虽然并不准确.

了解了基本的用途，来看看使用场景吧。我说说我一般的用法:

<!--more-->

1. `ciw`替换文本，然后在另一个也需要替换相同文本的单词处按下`.`，就可以执行相同的替换操作。
2. `dd`,`dw`,然后按下`.`就会再次删除一句或一个单词。我觉得连续输入`.`比数几秒有几个或几行比打`3dw/3dd`要更自然和更快，当然你觉得`3dw`更快也很正常.

这两个是我用的最多的，每天必用。大家有其他好用的欢迎补充。

今天还要介绍一个插件，叫做[repeat.vim](https://github.com/tpope/vim-repeat),和之前介绍过的surround.vim一样，来自tpope。repeat.vim做的事情很简单，重复一个插件操作，其中当然就支持surround.vim。这非常有用，比如给一个单词加了双引号，再按下`.`就可以也为另一个单词加上了。

repeat.vim还支持：

* [speeddating.vim](https://github.com/tpope/vim-speeddating)
* [abolish.vim](https://github.com/tpope/vim-abolish)
* [unimpaired.vim](https://github.com/tpope/vim-unimpaired)
* [commentary.vim](https://github.com/tpope/vim-commentary)

其中unimpaired.vim我也在用，下次可以介绍下。

今天就介绍到这里。