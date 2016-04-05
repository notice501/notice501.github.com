---
layout: post
title: "每日vim插件--显示git diff:GitGutter.vim"
date: 2014-04-21 20:44
comments: true
categories: 
---
周末休息了两天。今天继续。今天介绍的是一个可以显示git diff状态的插件——[vim-gitgutter](https://github.com/airblade/vim-gitgutter).

所谓显示diff状态，看一张图大家就明白了
![](http://ww1.sinaimg.cn/large/69d56e38gw1efni76kr62j20po0os78b.jpg)

<!--more-->

看最左边的标记列，一看就应该明白了。波浪线表示该行相比HEAD修改过，红色的减号表示这里删除了一行，绿色的+号表示这些行都是新增的。

这样git diff直接就一目了然，对自己的修改就更清晰了。

Gitgutter还支持在每个diff区块之间跳转（像图中就分了3块）。默认快捷键为`[c`和`]c`。可以非常方便地在各diff之间跳转了。

当然必须可以自定义mapping：

```vim
nmap ]h <Plug>GitGutterNextHunk
nmap [h <Plug>GitGutterPrevHunk
```

Gitgutter不仅能显示这些git diff，还能暂存`<Leader>hs`和回退`<Leader>hr`修改。

同样支持自定义mapping：
```vim
nmap <Leader>ha <Plug>GitGutterStageHunk
nmap <Leader>hu <Plug>GitGutterRevertHunk
```

不过这个我用的不是很多，暂时没感觉需要这样细粒度的执行暂存操作。

但是查看diff的修改我会比较常用，快捷键`<Leader>hp`,他会显示diff差异。如图所示：

![](http://ww3.sinaimg.cn/large/69d56e38gw1efnire8hi2j20va0po0yl.jpg)

gitgutter还支持自定义git diff的参数，比如：

```vim
let g:gitgutter_diff_args = '-w'
```

就介绍这么多了。btw，感觉用了gitgutter，Gundo的会变得略微慢一点。但是，Gitgutter显然是必备插件啊。有问题直接回复。
