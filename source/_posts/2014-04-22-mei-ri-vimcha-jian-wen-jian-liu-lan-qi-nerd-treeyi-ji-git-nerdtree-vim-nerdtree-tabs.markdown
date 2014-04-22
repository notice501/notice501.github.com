---
layout: post
title: "每日vim插件--文件浏览器NERD Tree以及git-nerdtree,vim-nerdtree-tabs"
date: 2014-04-22 19:43
comments: true
categories: [vim]
---
今天介绍一个必备插件——[NERD Tree]()。这个插件基本用vim的都会知道吧。看图就知道了：
![](http://ww2.sinaimg.cn/large/69d56e38gw1efoqvqpramj21gu12i46s.jpg)

用它可以让vim像其他大多数编辑器或者IDE一样，打开一个分割窗口显示文件树，在这个文件树上可以通过`j`,`k`上下浏览以及其他一些快捷键进行快速文件导航。按回车就直接打开该文件，当然还可以通过`i`,`s`,`t`在分隔窗口或者在新标签页打开.

<!--more-->

NERD Tree还支持书签。所谓书签，就是标记该文件或文件夹会优先显示在最上方。如前面图中的_posts文件夹。只要光标在该文件上输入命令`:Bookmark <name>`.非常方便

NERD Tree不想介绍过多，使用非常简单。在NerdTree窗口按下问号就会有帮助信息介绍所有的操作和对应的快捷键，常用下很快就很熟了。

NERD Tree有个问题在于和大多数IDE或者编辑器不同的是，在新的Tab中，NERD Tree默认是不打开的。不同的tab不能共享一个NERD Tree窗口，有人对这种方式可能就比较别扭。这时候就诞生了[vim-nerdtree-tabs](https://github.com/jistr/vim-nerdtree-tabs).该插件就解决了这个问题，它让每个tab都有相同的NERD Tree，看起来就像NERD Tree固定在最左一样。按下`:NERDTreeTabsToggle`就可以打开或关闭所有窗口。关闭文件窗口的时候，对于tab的NERD Tree窗口也会自动关闭。

接下来介绍的[git-nerdtree](https://github.com/Xuyuanp/git-nerdtree)是来自@Xuyuanp的作品,通过微信公众号介绍给我。也是对NERD Tree的增强修改，增加了文件的git状态显示，和昨天的gitgutter类似，Gitgutter显示文件内的git diff，而git-nerdtree为NERD Tree增加了文件git状态的显示：

![](https://camo.githubusercontent.com/3fe0388df11cb787f36e1fa108398fd3f757eef4/687474703a2f2f692e696d6775722e636f6d2f6a534377476a552e6769663f31)

相对应的标识如下：

* `✭` / `*` : Untracked
* `✹` / `~` : Modified in the working tree
* `✚` / `+` : Staged in the index (Exclude Renamed status)
* `➜` / `»` : Renamed
* `═` / `=` : Unmerged
* `✖` / `-` : Deleted (This indicator can't be shown, as NERDTree doesn't display deleted files. I have no prefect idea to solve this problem currently.)
* `✗` / `×` : Dirty (Only for directory)
* `✔` / `ø` : Clean (Only for directory)

标识方式和我之前介绍的zsh配置对git库状态的处理类似（可以查看我之前的博客）。

同时还支持和gitgutter一样的快捷键在各个有状态的文件之间跳转`[c`,`]c`.我只能说太棒了，再次感谢@Xuyuanp。

今天就介绍到这里，任何问题都可以直接回复。




