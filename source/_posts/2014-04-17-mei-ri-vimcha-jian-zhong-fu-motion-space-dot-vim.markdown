---
layout: post
title: "每日vim插件--可以重复motion的space.vim"
date: 2014-04-17 21:10
comments: true
categories: [vim]
---

<img src="http://ww3.sinaimg.cn/large/69d56e38gw1efiwusn0uhj20m80godg5.jpg" style="height: 400px;" />

今天头疼的厉害。不过昨天没更新，今天必须有了。介绍个简单点的。

vim中在普通模式下，空格这么大一按键不用有点浪费，所以有了[space.vim](https://github.com/spiiph/vim-space).

<!--more-->

他能重复执行motion，比如

	*Hello World
	
按下`fo`,会将光标移动到第一个o上，再按下空格，就能移动到第二个o了，他会重复执行上一个`fo`。按下`<Shift-Space>`反向执行改操作，也就是光标又会回到第一个`o`.

除此之外，space.vim还能重复搜索命令，diff移动命令，qucikfix等操作。让空格键变的非常有用.下面是space.vim可以重复的全部命令列表：

```
Character movements:                                    |left-right-motions|
    |f| |F| |t| |T| |;| |,|

Search commands:                                           |search-commands|
    |star| |gstar| |#| |g#| |n| |N|

Jump list jumps:                                              |jump-motions|
    |CTRL-O| |CTRL-I|

Change list jumps:                                       |change-list-jumps|
    |g;| |g,|

Diff jumps:                                                   |jumpto-diffs|
    |]c| |[c|

Parenthesis and bracket jumps:                             |various-motions|
    |])| |[(| |]}| |[{|

Method jumps:                                              |various-motions|
    |]m| |[m| |]M| |[M|

Section jumps:                                              |object-motions|
    |]]| |[]| |][| |[[|

Fold movements:
    |zj| |zk| |]z| |[z|

Tag movements:                                                |tag-commands|
    |CTRL-]|
    |:tag|
    |:tnext|
    |:tprevious|
    |:tNext|
    |:trewind|
    |:tfirst|
    |:tlast|

Undolist movements:                                          |undo-branches|
    |g-||g+|

Quickfix commands:                                                |quickfix|
    |:make|
    |:vimgrep|
    |:grep|
    |:cc|
    |:cnext|
    |:cprevious|
    |:cNext|
    |:cfirst|
    |:clast|
    |:crewind|
    |:cfile|
    |:cnfile|
    |:cpfile|
    |:cNfile|

Location list commands:                                      |location-list|
    |:lmake|
    |:lvimgrep|
    |:lgrep|
    |:ll|
    |:lcnext|
    |:lcprevious|
    |:lcNext|
    |:lcfirst|
    |:lclast|
    |:lcrewind|
    |:lcfile|
    |:lcnfile|
    |:lcpfile|
    |:lcNfile|
```

今天解介绍到这里。
