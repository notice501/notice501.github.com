---
layout: post
title: "每日vim插件--缩进显示vim-indent-guides"
date: 2014-04-11 17:49
comments: true
categories: [vim]
---
今天有朋友留言问昨天晒配色的图中缩进用的什么插件，那今天就介绍这个缩进插件——[vim-indent-guides](https://github.com/nathanaelkane/vim-indent-guides)

选择这个插件主要有几个理由：

1. 插件对tab和空格的支持都很好。
2. 比较美观。
3. 能够自动适配当前使用的colorscheme来选择缩进颜色（只能是gvim，macvim下适配的不错）

默认的快捷键是`<Leader>ig`,开关插件。我一般都默认启动就开启,只要设置：

```vim
let g:indent_guides_enable_on_vim_startup = 1
```

前面说了缩进的颜色是自动选择的，非常方便，但是想要自定义颜色也是支持的：

```vim
let g:indent_guides_auto_colors = 0
autocmd VimEnter,Colorscheme * :hi IndentGuidesOdd  guibg=red   ctermbg=3
autocmd VimEnter,Colorscheme * :hi IndentGuidesEven guibg=green ctermbg=4
```

在终端中该插件就不支持颜色自动选择了。只取决于`background`设置为`dark`还是`light`,如果设置了`dark`,就相当于配置了

```vim
hi IndentGuidesOdd  ctermbg=black
hi IndentGuidesEven ctermbg=darkgrey
```
来几张图，图上标注了相应的配置：

![](http://ww4.sinaimg.cn/large/69d56e38gw1efbth8m9aij20cg0cgta5.jpg)
![](http://ww2.sinaimg.cn/large/69d56e38gw1efbtngrerjj20cg0cg3zt.jpg)
![](http://ww2.sinaimg.cn/large/69d56e38gw1efbtqa6k47j20cg0cgjsq.jpg)

这个插件并不能很好的标记出tab和空格混用的情况，只能显示当前缩进。所以我在我的vimrc中加了一行：
```
" highlight tabs and trailing spaces
set list
set listchars=tab:>-,trail:-,extends:>,precedes:<
```
这样tab会被显示为>-，而尾部空格被显示为-，这样写出来的代码就不会再有杂乱符号啦。

今天就介绍到这里。我开通了一个微信公众账号，以后每天的文章会通过微信公众账号推送，欢迎关注。