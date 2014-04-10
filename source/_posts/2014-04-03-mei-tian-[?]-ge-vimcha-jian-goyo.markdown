---
layout: post
title: "每天一个vim插件--goyo"
date: 2014-04-03 21:50
comments: true
categories: [vim]
---
一如既往很忙……所以今天还是分享一个可以一句话说明白的插件，也是我非常常用的--[goyo](https://github.com/junegunn/goyo.vim)

一直非常喜欢写作软件iawriter，goyo让我可以用同样的方式来写代码，当然goyo还免费开源，这比iawriter好。用作者的描述就是：Distraction-free writing in Vim. 不会被任何的其他元素打扰。
<!--more-->
直接上张图：![](http://ww4.sinaimg.cn/large/69d56e38gw1ef2r2wod9uj21kw0zkte6.jpg)
![](https://raw.github.com/junegunn/i/master/goyo.png)

第一张是我的配色，第二张是作者的示例。是不是很cool？

安装当然借用vundle了。

	nnoremap <Leader>d :Goyo<CR>

就可以按引导键加逗号呼出goyo模式了。
可以按自己的需求配置宽高和位置：

- `g:goyo_width` (default: 80)
- `g:goyo_margin_top` (default: 4)
- `g:goyo_margin_bottom` (default: 4)
- `g:goyo_linenr` (default: 0)
- `g:goyo_callbacks` ([before_funcref, after_funcref])

goyo模式中默认禁用了
[vim-airline](https://github.com/bling/vim-airline),
[vim-powerline](https://github.com/Lokaltog/vim-powerline),
[powerline](https://github.com/Lokaltog/powerline),
[lightline.vim](https://github.com/itchyny/lightline.vim), and
[vim-gitgutter](https://github.com/airblade/vim-gitgutter)插件。如果需要自定义goyo模式或者一些插件的enable/disable，

可以在vimrc中定义before和after回掉：

```vim
function! s:goyo_before()
  silent !tmux set status off
  set noshowmode
  set noshowcmd
  " ...
endfunction

function! s:goyo_after()
  silent !tmux set status on
  set showmode
  set showcmd
  " ...
endfunction

let g:goyo_callbacks = [function('s:goyo_before'), function('s:goyo_after')]
```

可以在[这里](https://github.com/junegunn/goyo.vim/wiki/Customization)看到更多的自定义示例


好吧，基本就是翻译了一下……因为插件很简单，文档也很详细。我自己非常喜欢这个插件，希望大家也会喜欢～


