---
layout: post
title: "每日vim插件--vim 撤销树Gundo.vim"
date: 2014-04-15 21:10
comments: true
categories: [vim]
---

今天介绍的插件很有意思，也非常有用，也是我最常用的插件之一——[Gundo](https://github.com/sjl/gundo.vim).

大家都知道按`u`可以撤销操作，但是一般都不知道输入命令`:undolist`会显示可撤销列表，如图

<img src="http://ww1.sinaimg.cn/large/69d56e38gw1efglaef02lj20ki13qthg.jpg" style="
    height: 600px;
"/> 

其实这还不是列表，而是整个vim 撤销树的叶子。为什么说是树，而不是列表，举个例子就明白了：

<!--more-->

你在a状态做了一次修改到b，又回退到a，再做了一次修改到c。大多数编辑器比如sublime text，b这个状态就没了，但是vim会用一个树进行保存。

而Gundo这个插件就是一个撤销树浏览器.直接上张图：

<img src="http://ww2.sinaimg.cn/large/69d56e38gw1efgmev2lz4j20ok1ci78i.jpg" style="
    height: 600px;
"/>

当前位置以`@`标注，其他历史以o标注。

按jk上下移动，就可以在下面的窗口看到对应修改之前的改动。这个就是普通的vim窗口，所有的移动操作都是支持的，比如`G`到底部，`C-U`上翻页等。

按`p`可以查看选中状态和当前状态的差异，按回车就会回到选中状态，按`P`更是可以一步步播放到选中状态，高上大啊……

btw,我习惯将所有的undo记录都保存下来，即使关闭了vim或者buffer也能继续撤销。

只需要稍加配置，就能将撤销树持久化存储下来：

```vim
try
    set undodir=~/.vim/temp_dirs/undodir
    set undofile
catch
endtry
```

今天就介绍到这里。有问题直接回复给我。




