---
layout: post
title: "每日vim插件--快速选择文本对象wildfile.vim"
date: 2014-04-18 20:06
comments: true
categories: [vim]
---

之前我们介绍了文本对象，并介绍了两个自定义文本对象的插件。今天介绍的插件也和文本对象有关。他可以用来快速的就近选择一个候选文本对象，并能通过快捷键继续简单的扩大文本对象范围。

这个插件就是--[wildfire](https://github.com/Shougo/wildfire.vim)（给的是shougo fork的repo地址，比较喜欢shougo这个插件作者，而且这个fork fix了一个bug）

插件默认定义的候选文本对象为：

	 `i'`, `i"`, `i)`, `i]`, `i}`, `ip` and `it`

来一张官方图

<!--more-->

![](http://ww1.sinaimg.cn/large/69d56e38gw1efk07xfk7kg20jv087myy.gif)

有图应该大家能理解这个插件的作用了。

使用方式也非常简单，按`enter`选择最近一个文本对象，再按下`enter`在刚刚的选择之上再选择最近的文本对象。按下`<BS>`就能回退到上一个选择。

当然，你可以自定义快捷键：

```vim
" This selects the next closest text object.
let g:wildfire_fuel_map = "<ENTER>"

" This selects the previous closest text object.
let g:wildfire_water_map = "<BS>"
```

候选的文本对象也是可以配置的：

```vim
let g:wildfire_objects = ["i'", 'i"', "i)", "i]", "i}", "ip", "it"]
```

插件还支持对文件类型分别定义，比如要在html中只处理tag，忽略其他文本对象，只需要配置：

```vim
" use '*' to mean 'all other filetypes'
" in this example, html and xml share the same text objects
let g:wildfire_objects = {
    \ "*" : ["i'", 'i"', "i)", "i]", "i}", "ip"],
    \ "html,xml" : ["at"],
\ }
```

与之类似的一个用的相当广泛的插件还有[vim-expand-region](https://github.com/terryma/vim-expand-region)。大家可以自己选择。有问题欢迎留言或回复交流。也希望大家推荐自己在用，自己写的插件给我。今天看到一个朋友微信回复我的一个他自己写的插件，我已经立刻用上了，卖个关子，下次介绍。


