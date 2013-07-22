---
layout: post
title: "我在用的mac软件(2)-终端环境之zsh和z(*nix都适用)"
date: 2013-07-18 20:53
comments: true
categories: [mac,zsh,shell]
---

继续上篇介绍我的终端环境。这篇介绍zsh和z，其实这不局限于os x，在所有的*nix系统中都是可用的。

# zsh

 zsh作为bash的替代品，自然很多人要问：why zsh？
 在[Zsh Workshop](http://www.acm.uiuc.edu/workshops/zsh/why.html) 有个长长的功能列表，用来回答这个问题。这里讲下我选择zsh的原因，当然，也是介绍zsh强大的功能。
 
 1. 兼容bash。这使得切换到zsh没有任何成本。
 2. OS X默认的bash版本实在是太老了啊……
 3. 拼写纠正。你总会不小心打错命令。这时，zsh会进行自动拼写纠正，如图：![](http://foocoder.com/images/mac/cerrect.png)
 4. 更强大的补全。
 <!--more-->
   * 连按两次tab会列出所有的补全列表并直接开始选择。如图：![](http://foocoder.com/images/mac/tabcd.png)
 	并且可以用方向键来选择，但是对我这种很少用方向键的人来说只能猛敲tab了么，不是，zsh支持使用`<ctrl-n/p/f/b>`来选择，perfect!
   * 命令选项补全。有多少人依然记不住tar的命令选项？中枪的去抄20遍……在zsh中只需要键入`tar - <tab>`就会列出所有的选项和帮助说明。用了zsh之后`man`少用了好多……
   * 命令参数补全。zsh 对命令的参数补全也很强大。键入`kill <tab>`就会列出所有的进程名和对应的进程号。如图：![](http://foocoder.com/images/mac/kill2.png)这还不够，试试键入`kill sbin <tab>`,如图所示:![](http://foocoder.com/images/mac/kill1.png)自动为sbin这个进程名补全了进程号。kill进程再也不用两步操作了。
 5. 更智能的历史命令。在用<ctrl-p>或者`方向上键`查找历史命令时，zsh支持限制查找。比如，输入`ls `然后再按方向上键,则只会查找用过的ls命令。而此时使用`<ctrl-p>`则会仍然按之前的方式查找，忽略`ls`。
 6. 多个终端会话共享历史记录。经常有多个窗口，tab，tmux的多个session，panel。这些命令历史不能共享实在是很糟糕的回忆。但是有了zsh之后，这些确实成了回忆了,所有的命令历史都可以共享。
 7. 更智能的`cd`。首先你甚至不需要再输入cd了，直接输入路径即可。第二，在你知道路径的情况下，比如`/usr/local/bin`你可以输入`cd /u/l/b`然后按`<Tab>`进行补全快速输入。这显然不够，zsh还支持路径替换，如果你其实想进入的是`/usr/local/bin`，不再需要`../` 了，直接在当前输入`cd bin share`即可，则`bin`会替换为`share`。在之后我会介绍z和autojumper，目录跳转会更方便。 
 8. 更强大的alias。zsh不仅支持普通的alias，例如：`alias ls ='ls --color=auto'`。zsh还支持后缀alias,即以什么命令打开特定的后缀名文件。例如`alias -s js=mvim`,输入`hello.js`，会以vim打开该文件,而不在需要`vim hello.js`。
 9. 通配符搜索。这也是我最爱的功能之一。之前讲过由于命令补全少用了很多`man`命令，而这个功能让我少用了很多`find`命令。	一般的通配符搜索无非是`ls -l *.log`,如图:![](http://foocoder.com/images/mac/ls1.png)在zsh中可以做到递归的通配符搜索。使用`**/`来递归搜索，如图![](http://foocoder.com/images/mac/ls2.png)是不是在很多场景下可以取代`find`？
 
 
以上都是我感觉迁移到zsh之后非常实用的功能。要想从头开始了解和学习zsh，可以访问[A User's Guide to the Z-Shell](http://zsh.sourceforge.net/Guide/zshguide.html)。不过大家都很忙，从头开始自己学习和配置会很累。所以现在说到zsh，不得不提的就是[oh-my-zsh](),oh-my-zsh是一个开源的zsh配置管理框架，提供了大量实用的功能，主题等。现在基本都是标配了吧。如图是我在用的默认zsh主题`robbyrussell`，如图![](http://foocoder.com/images/mac/gitoh.png)可以发它能自动显示当前所在的git分支以及当前本地状态（黄色的小叉表示本地有更新未提交）。

当然zsh也不是完美无缺。在我使用过程中有两点不是很舒服：
1. 自动纠正并不总是那么智能。如图：![](http://foocoder.com/images/mac/wrongcorrect.png)
2. 一些符号是zsh中保留的，使用需要转义，如图：![](http://foocoder.com/images/mac/zhuanyi.png)
 
 ---
 
 下面讲下zsh和oh-my-zsh的安装。
 
__使用brew来安装zsh__
 
 ```
  brew install zsh
 ```
 
 __设置zsh为默认__
 
 在`/etc/shells`文件末尾添加
 
 ```
 /usr/local/bin/zsh
 ```	
 
 执行：
 
 ```
 chsh -s /usr/local/bin/zsh
 ```
 
 最后记得将`~/.bash_prorile`或者`~/.profile`等配置拷贝到~/.zshrc中。
 
 __安装oh-my-zsh__

 自动安装:
 
``` 
curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh
```
 
可以选择自己喜欢的[主题](https://github.com/robbyrussell/oh-my-zsh/wiki/themes)。只需要修改`~/.zshrc`文件中的`ZSH_THEME`即可。
 
# z和autojump

[z](https://github.com/rupa/z)和[autojump](https://github.com/joelthelion/autojump)的功能类似，前者是简单的shell脚本实现，后者由python实现，功能都是可以方便自动匹配到你最多使用的目录并跳转。我在用的是z，如图，我在根目录输入`z github`可以自动跳转到我常用的`notice501.github.com`这个目录，也就是本博客的工程目录。超级方便的工具。autojump用法类似，命令为`j`而不是`z`两者的安装方式：

__z__

```
git clone git@github.com:rupa/z.git
```

而后将z.sh放入环境变量即可。

__autojump__

autojump可以直接使用brew安装

```
brew install autojump
```

有问题和分享欢迎留言交流。也欢迎关注我的[微博](http://weibo.com/notice520)。
 
 
 
 	
 	
 
 
 
 	
  