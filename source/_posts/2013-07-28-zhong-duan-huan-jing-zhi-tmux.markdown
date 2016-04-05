---
layout: post
title: "终端环境之tmux"
date: 2013-07-28 17:09
comments: true
categories: [shell,tmux,mac]
---
今天继续介绍我的终端环境，tmux。
------------

tmux 是一个优秀的终端复用软件，类似 GNU Screen，但来自于OpenBSD，采用BSD授权。使用它最直观的好处就是，通过一个终端登录远程主机并运行tmux后，在其中可以开启多个控制台而无需再“浪费”多余的终端来连接这台远程主机。简单来说，tmux是一个`multiplexers`,他可以让你同时运行多个终端，在多个终端之间切换。

##why tmux?
用一个工具的第一问自然还是为什么要用。其实当时使用tmux的原因很简单。工作中经常需要长时间的编译。在想要回家时编译还没结束，可以在计算机休眠的情况下继续编译。简单的寻觅一番之后，就发现了tmux。而且远超预期，就一直用了下来。


其他让我非常喜欢的功能有：

<!--more-->

1. window，pane的概念可以很好地进行多窗口切换，窗口分割。
2. 状态行配置很容易。
3. vi 模式
4. 复制粘贴缓冲区
4. 脚本化.通过脚本可以自动化窗口布局。

## tmux简单介绍

tmux是典型的c/s架构。有如下几个概念。

- session. session是一个特定的终端组合。输入tmux就可以打开一个新的session。
- window。window 为session中的终端。
- pane 。pane为一个window分隔出来的各个间隔，即window中的终端。


##tmux的使用
正如上所述，在终端中输入`tmux`就可以打开一个tmux session。如图：
![](http://foocoder.com/images/mac/tmux.png)

底部会出现状态栏。左边表示当前为session 0， window 1， pane 1，中间会显示当前窗口编号和路径，右侧会本机信息和时间。这并不是默认设置，但是配置tmux的状态行非常容易，在后面我会简单的介绍如何配置tmux，并提供我的配置文件。

tmux的所有操作必须先使用一个前缀键进入命令模式，或者说进入控制台，就像vi中的`esc`。默认的前缀为`<c-b>`,比较难按，很多人会改为screen中的`<c-a>`，来保持一致性。可是这和emacs风格的终端回到行首的快捷键冲突，我使用的是`c-k`。大家可以根据自己喜好来配置：

```
set -g prefix ^k
unbind ^b
```
输入`?`显示所有的bind-key，如图![](http://foocoder.com/images/mac/allbindkey.png)

如果设置了`setw -g mode-keys vi`,可以使用vi 的 `j` `k`风格快捷键上下浏览。这些bind-key显示了所有的tmux操作。按q退出。

下面就介绍一些常用的操作，为了方便大家查看，所有的bind-key都是系统默认的，而不是我自己配置的。省略了前缀键。

###复制粘贴
- `[` 进入复制模式。
- `]` 粘贴

进入复制模式后，可以用vi风格的快捷键进行移动（按上文的设置）。按下`sapce`就可以选择文本。回车键进行复制。然后再通过`]`进行粘贴。

这里我将复制粘贴设为类似vi的模式,使用`esc`进入复制模式，`v`进入粘贴模式，选择后`y`进行复制。`<Prefix-p>`进行粘贴.

```
# Copy and paste like in vim
unbind [
bind Escape copy-mode
unbind p
bind p paste-buffer
bind -t vi-copy 'v' begin-selection
bind -t vi-copy 'y' copy-selection
```

所有的复制都会被记录到缓冲区，输入`#`或者 `tmux list-buffers`查看缓冲区,同时也进入了复制模式。也可以使用"="来选择并粘贴缓冲区内容。tmux的缓冲区和系统剪贴板是完全独立的，为了复制到系统剪贴板，我做了如下处理，对于mac os X用户：

第一步：

```
brew install reattach-to-user-namespace
```

而后增加配置：

```
# getting tmux to copy a buffer to system clipboard
set-option -g default-command "reattach-to-user-namespace -l zsh" # or bash...
bind y run "tmux save-buffer - | reattach-to-user-namespace pbcopy" \; display-message "Copied tmux buffer to system clipboard"
bind C-v run "reattach-to-user-namespace pbpaste | tmux load-buffer - && tmux paste-buffer"
```

这样，在tmux中进行复制后。按下前缀键后键入`y`,就会在状态栏显示已粘贴到剪贴板，如图![](http://foocoder.com/images/mac/paste.png)
此时，就可以用`cmd-v`进行粘贴了。系统剪贴板的也可以键入`<C-v>`粘贴。（当然，更方便的还是直接`cmd-v`,无需前缀键）。

对于linux用户,可以使用xclip,配置如下：

```
bind y run-shell "tmux show-buffer | xclip -sel clip -i" \; display-message "Copied tmux buffer to system clipboard"
```
同样键入`y`复制buffer中最新的内容到系统剪贴板。



###session操作
- `d` deattch当前session。输入`tmux attach [-t sessionname]`重新进入该session。
- `tmux ls` 列出所有session。如图:![](http://foocoder.com/images/mac/allsession.png)输入，退出当前session后，`tmux attach -t 1`即可切换到名字为1的session。
- `$` 重命名当前session
- `<c-z>` 挂起当前session


###window操作
- `c` 创建一个新的window
- `b` 重命名当前window
- `&` 关闭当前window
- `n` 移动到下一个窗口
- `p` 移动到前一个窗口
- `l` 切换到上一个窗口
- `w` 列出所有窗口编号,并进行选择切换
- `编号` 移动到指定编号的窗口。
- `.` 修改窗口编号，相当于排序。
- `f` 搜索所有的窗口。非常方便的功能。如图![](http://foocoder.com/images/mac/search.png)

####pane操作
- `"` 横向分割
- `%` 纵向分割
- `方向键` 在pane直接移动
- `o` 到下一个pane
- `opt+方向键` 调整pane大小
- `{ / }`左右pane交换
- `空格` 横竖切换
- `q` 显示pane的编号
- `x` 关闭当前pane

我的配置将分割操作改为vi风格的`v`和`s`,而pane之间的跳转也改为vi风格,调整pane的大小也是一样。配置如下：

```
# split windows like vim.  - Note: vim's definition of a horizontal/vertical split is reversed from tmux's
unbind '"'
unbind %
unbind s
bind s split-window -v
bind S split-window -v -l 40
bind v split-window -h
bind V split-window -h -l 120

# navigate panes with hjkl
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# resize panes like vim
bind < resize-pane -L 10
bind L resize-pane -L 100
bind > resize-pane -R 10
bind R resize-pane -R 100
bind - resize-pane -D 5
bind D resize-pane -D 36
bind + resize-pane -U 5
bind U resize-pane -U 35

# swap panes
bind ^u swapp -U
bind ^d swapp -D
```

同时还绑定了

```
bind q killp
```
使用`q`来关闭pane，免去了关闭确认.但是会覆盖掉原有的`p`操作，显示pane编号。我不需要这个，覆盖就覆盖了。


### 脚本化tmux
tmux可以进入命令行模式，快捷键为`:`。而且运行的命令在不同的session中都会生效。我绑定了一个命令：

```
bind r source-file ~/.tmux.conf \; display "Configuration Reloaded!"
```
这样只要输入`r`,就可以重新加载tmux.conf配置文件。

还可以用来进行自动化布局，例如：

```
selectp -t 0             
splitw -h -p 50           
selectp -t 1              
splitw -v -p 40 'node'  
selectp -t 2              
```

将其保存在随便在一个文件中，而后使用和上述类似的`source-file`加载该文件，就会分隔三个pane，其中一个pane 会输入node，开启一个node的js shell。其中的50，40 为占窗口大小的百分比。

同时，tmux还支持运行shell脚本。可以写一个shell脚本进行各种环境初始化和布局初始化。这里就不再介绍了。

### 状态栏

tmux的状态栏配置非常简单。相比screen就…… 比如我的配置中：

```
set -g status-left "#[fg=green]s#S:w#I.p#P#[default]"
```

这一行就将状态栏左侧配置为：![](http://foocoder.com/images/mac/statusbar.png)

绿色，#S,#I,#p分别表示session，window，pane编号。

当然，你可以让状态行更强大,可以使用[tmux-powerline](https://github.com/erikw/tmux-powerline)。
如图![](http://foocoder.com/images/mac/powerline-status.png)

是一个示例样式。
不过我还是喜欢简洁，而且大多数的信息其实都没什么用。自己并没有使用，不过还是推荐大家试一试，使用也不复杂，按照说明一步步来就可以了。使用powerline需要使用pathc过的字体，在[这里](https://github.com/Lokaltog/powerline-fonts)可以找到一些，当然也可以自己patch。

--------------
最后提供我的整个配置文件，可以在我的[dotfiles](https://github.com/notice501/dotfiles)的tmux目录下找到。

欢迎留言交流。也可以关注我的微博**[foocoder](http://weibo.com/notice520)**。




