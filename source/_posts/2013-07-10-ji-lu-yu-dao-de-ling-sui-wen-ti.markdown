---
layout: post
title: "记录遇到的零碎问题"
date: 2013-07-10 20:47
comments: true
categories:
---
日常开发过程中总会遇到各种各样奇怪的问题。相信大家都会有这样的体会：如果有人也遇到过这个问题，并分享了解决过程，自己就会很开心。如果这个问题没人分享过，那恐怕就要费劲一番周折了。这里记录那些各种小的莫名的琐碎的问题。方便自己查阅，也方便他人。会有大致分类，也会不定期更新。

### vim
 * 在使用了session的情况下，neocomplete对于session中加载的文件无法正常使用。原因是其completefunc为空。解决办法是在session加载之前，执行vimrcneocomplete#initialize()。（在vimrc中加入execute neocomplete#initialize()即可）
 <!--more-->
 
 * 在mac中alt的map会有问题。原因是alt加某个字符都会代表新的字符，比如 ,在我的mac中alt-1/2/3分别是¡/™/£。所以要map alt-1，只要map ¡即可。
 
 * macvim获取的 $PATH并不是用户常规设置的 $PATH。这会导致一些在 $PATH中加入的执行文件在macvim中无法执行，比如sytanstic众多的语法检查。这可以说是macvim的一个bug。相反，macvim使用的是/usr/libexec/path_helper，它使用两个部分的内容，一部分是/etc/paths，另一部分是/etc/paths.d。所以只要修改etc/paths.d，加入所需的环境变量即可。
 
 * 在终端中用mvim打开文件会直接在新窗口打开，可以加入–remote-tab参数在新标签打开。我还是喜欢在新标签打开，每次输入这个参数太麻烦。修改mvim执行文件，在头部加入tabs=true，并将#last step的if块替换为：
 
``` 
if [ "$gui" ]; then
 	if $tabs && [[ `$binary --serverlist` = "VIM" ]]; 	then
   	 exec "$binary" -g $opts --remote-tab-silent 	${1:+"$@"}
else    exec "$binary" -g $opts ${1:+"$@"}
	fi
else  exec "$binary" $opts ${1:+"$@"}
	fi
```
 
### zsh
* zsh中使用一些符号需要加上转义字符。比如rake new_post\["记录遇到的零碎问题"\],git reset HEAD\^,否则会报zsh: no matches found:错误。

### Parallels
* Parallels中登陆windows时提示密码过期，需要更高。但是我根本没设置过密码。原因可能是安装时跳过了设置。只需要保持原密码字段为空，键入新密码设置密码即可。

