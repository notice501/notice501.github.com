---
layout: post
title: "每日vim插件--命令行补全cmdline-completion"
date: 2014-04-23 21:57
comments: true
categories: [vim]
---
今天介绍一个非常实用简单的插件，叫做[cmdline-completion](https://github.com/vim-scripts/cmdline-completion).功能就和名字描述的一样，在输入命令的时候，提供补全功能。vim自身对一些命令有补全功能，该插件对其进行了增强。比如：

 1.   :something<C-P>
 2.   /else<C-N>
 
 也可以自定义快捷键进行补全：
 
 ```vim
 cmap <C-J> <Plug>CmdlineCompletionBackward 
 cmap <C-K> <Plug>CmdlineCompletionForward
 ```
 
 今天的介绍非常简短，希望大家喜欢。有问题可以回复给我。