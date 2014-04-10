---
layout: post
title: "每天一个vim插件--开篇之vundle"
date: 2014-04-01 22:34
comments: true
categories: [vim]
---
一直想写一篇博客介绍我的vim配置和插件，但是这篇博客却难产了快半年……
工作太忙，写博客变成了奢侈的事情。那何不每天写一点点呢？于是决定每天介绍一个vim插件或者一个技巧。

大致看了一下自己的[vim配置](https://github.com/notice501/dotfiles)，竟然都快有100个插件之多了……

但是我从来都没有感觉到插件管理有多麻烦，我可以经常更新，删除和安装想用的插件。所以第一个介绍的插件必须是用来管理插件的神器--[Vundle](https://github.com/gmarik/Vundle.vim)

在使用vundle之前，我使用Pathogen与git submodule来管理Vim插件，而vundle更为强大，不需要再手动操作git了。Vundle会自动去对应的插件git库获取最新的插件。
<!--more-->

Vundle的安装非常简单：

1. 当然你需要安装git
2. git clone https://github.com/gmarik/vundle.git ~/.vim/bundle/vundle
3. 配置vimrc。我建议像我一样单独写个bundles.vim，(我还是用的Bundle这个名字，但是写这篇博客的时候发现作者已经废弃了这个名字，统一叫做plugin)方便管理。示例如下：

```vim
	 set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/vundle/
call vundle#rc()
" alternatively, pass a path where Vundle should install plugins
"let path = '~/some/path/here'
"call vundle#rc(path)

" let Vundle manage Vundle, required
Plugin 'gmarik/vundle'

" The following are examples of different formats supported.
" Keep Plugin commands between here and filetype plugin indent on.
" scripts on GitHub repos
Plugin 'tpope/vim-fugitive'
Plugin 'Lokaltog/vim-easymotion'
Plugin 'tpope/vim-rails.git'
" The sparkup vim script is in a subdirectory of this repo called vim.
" Pass the path to set the runtimepath properly.
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" scripts from http://vim-scripts.org/vim/scripts.html
Plugin 'L9'
Plugin 'FuzzyFinder'
" scripts not on GitHub
Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
Plugin 'file:///home/gmarik/path/to/plugin'
" ...

filetype plugin indent on     " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList          - list configured plugins
" :PluginInstall(!)    - install (update) plugins
" :PluginSearch(!) foo - search (or refresh cache first) for foo
" :PluginClean(!)      - confirm (or auto-approve) removal of unused plugins
"
" see :h vundle for more details or wiki for FAQ
" NOTE: comments after Plugin commands are not allowed.
" Put your stuff after this line
```

然后在vimrc的开头引入bundles.vim:
	
	source ~/.vim/bundles.vim

4.如示例所示，将所有的插件都写成plugin 'user/repo'即可。vundle会从该库中去取。


如果未加'/'，则默认从vim script: https://github.com/vim-scripts/ 去取
	还可以加上非github库：
		
	Plugin 'git://git.wincent.com/command-t.git'
		
	或者本地文件
	
	Plugin 'file///path/from/root/to/plugin'
		
		
5.安装只需输入

	:BundleInstall 或者 :pluginInstall
	
更新：
	
	:pluginUpdate
		
删除：
	
	:pluginClean
		
6.vundle 还带了插件搜索功能

	:PluginSearch foo
	
搜索结果会在新窗口打开，然后可以进行直接安装删除等操作。
	
-------------	
从Vundle开始，享受vim丰富的插件带来的爽快感吧~