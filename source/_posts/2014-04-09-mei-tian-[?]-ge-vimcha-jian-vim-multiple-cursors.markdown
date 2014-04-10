---
layout: post
title: "每天一个vim插件--vim-multiple-cursors"
date: 2014-04-09 22:19
comments: true
categories: [vim]
---

前几天清明休假了。每日插件也就休息了几天。不过休假搞的比上班还累……

今天介绍一款我用的非常多，也非常有用的插件--[vim-multiple-cursors]()。

这个插件copy了sublime text的多重光标选取功能，非常强大。

sublime text 官网有几张图来介绍sublime text的多重选取功能，插件作者也实现了一样的效果：
![](http://ww1.sinaimg.cn/large/69d56e38gw1ef9pr4t1i2g20k406ojtw.gif)

<!--more-->

上图按键：

- fp跳到p处
- 按下`<C-n>`选中光标下的单词
- 继续按下`<C-n>`两次选中另外两个相同的单词
- 按下c进行修改
- 键入修改
- 按下 `<Esc>` 退出

![](http://ww1.sinaimg.cn/large/69d56e38gw1ef9pzm3d13g20sy0900zt.gif)


上图按键：

- 按下V选中整行
- 按下G到达末行
- 按下`<C-n>` 在每行的开头加上一个光标并返回普通模式
- 按下I在每行的头部插入
- 键入", 按下`<C-e>`到达行末, 键入另一个"和逗号
- 然后将每个光标都下移一行，按下delete	


再也不用羡慕sublime了。

使用也非常简单，几乎0配置。

默认的mapping：

```
" Default mapping
let g:multi_cursor_next_key='<C-n>'
let g:multi_cursor_prev_key='<C-p>'
let g:multi_cursor_skip_key='<C-x>'
let g:multi_cursor_quit_key='<Esc>'
```
在普通模式下，按下`Ctrl-n`开始进入可视模式并选中光标下的单词，继续按`Ctrl-n`选择下一个相同的单词，按下`Ctrl-p`往回选一个，`Ctrl-x`则跳过下一个相同单词。

选中后就可以对单词进行批量改动了，比如按下c，就同时修改选中单词。

插件还支持正则匹配，不过要用到正则去匹配的时候我就用%s来替换了。要了解详情可以去插件的github页继续了解。

今天就介绍这个性感无比的插件。