---
layout: post
title: "每日vim插件--有道翻译"
date: 2014-04-02 21:34
comments: true
categories: [vim]
---
今天头疼。简短介绍一个实用又好用的插件，在vim中直接进行有道翻译,[vim-youdao-translater](https://github.com/ianva/vim-youdao-translater)，来自ianva.我非常喜欢的一个插件,在这里再次感谢作者ianva。

安装就不介绍了，不知道怎么安装看上一篇博客。

使用方式copy自ianva：

在普通模式下，按 ctrl+t， 会翻译当前光标下的单词；

在 visual 模式下选中单词，按 ctrl+t，会翻译选择的单词；

点击引导键再点y，d，可以在命令行输入要翻译的单词；

译文将会在编辑器底部的命令栏显示。 

上述操作的配置：

	vnoremap <silent> <C-T> <Esc>:Ydv<CR> 
	nnoremap <silent> <C-T> <Esc>:Ydc<CR> 
	noremap <leader>yd :Yde<CR>
	
明天继续。