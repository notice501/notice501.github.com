---
layout: post
title: "每日vim插件--Gist.vim"
date: 2014-04-25 19:58
comments: true
categories: [vim]
---
![](http://ww2.sinaimg.cn/large/69d56e38gw1efs6kvu5zpj20m80go3z0.jpg)
今天介绍的插件[Gist.vim](https://github.com/mattn/gist-vim)能够在vim中方便的创建和查看gist。

gist我就不过多介绍了，github提供的一个代码片段托管服务。不太了解的同学可以看看[这个教程](http://www.worldhello.net/gotgithub/06-side-projects/gist.html)

要使用这个插件需要安装ygie依赖插件：

```vim
Bundle 'mattn/webapi-vim'
Bundle 'mattn/gist-vim'
```

确保在git的global配置中设置的是github用户名：

	$ git config --global github.user <username>

该插件在首次使用时会需要你输入github密码来获取token，并将其保存在`~/.gist-vim`.

<!--more-->

使用非常的简单，输入命令

```
:Gist
```

就会将该整个文件创建一个Gist，创建成功后会显示Gist地址，如图：
![](http://ww3.sinaimg.cn/large/69d56e38gw1efs60clnedj20my040gm2.jpg)

也可以选中一段代码创建Gist：

```
 :'<,'>Gist
```

还提供了一些参数

例如`-a`,表示匿名创建，`-p`创建pravite 的gist，`-P`创建public的gist。`-m`为所有打开的buffer创建Gist。

除此之外，还可以编辑Gist(已打开了一个gist buffer的情况下)

```
:Gist -e
```
加上描述

```
:Gist -s something
```

删除：

```
:Gist -d
```

fork:

```
:Gist -f
```

star:

```
:Gist +1
```

unstar:

```
:Gist -1
```

还可以直接取得Gist：

```
:Gist XXXXX
```

一般你不记得gist号码，没关系，还可以列出所有的Gist,

```
:Gist -l
```

这样会打开一个新的分隔窗口显示你已有的gist列表，按回车就可以直接去取这个gist并在vim中查看了。

还提供了一些非常有用的配置，比如：

如果你想要在创建了gist后立刻打开浏览器查看：

```vim
let g:gist_open_browser_after_post = 1
```

如果想要默认创建的gist不是public而是private：

```vim
let g:gist_post_private = 1
```

有了这个插件，玩转gist是不是非常easy啦。这个插件唯一的不足在于在创建gist或者请求gist时会阻塞界面，这个有点糟糕。