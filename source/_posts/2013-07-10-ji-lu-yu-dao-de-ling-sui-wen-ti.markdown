---
layout: post
title: "记录遇到的零碎问题"
date: 2013-07-10 20:47
comments: true
categories:[]
---
日常开发过程中总会遇到各种各样奇怪的问题。相信大家都会有这样的体会：如果有人也遇到过这个问题，并分享了解决过程，自己就会很开心。如果这个问题没人分享过，那恐怕就要费劲一番周折了。这里记录那些各种小的莫名的琐碎的问题。方便自己查阅，也方便他人。会有大致分类，也会不定期更新。

### vim
 * 在使用了session的情况下，neocomplete对于session中加载的文件无法正常使用。原因是其completefunc为空。解决办法是在session加载之前，执行vimrcneocomplete#initialize()。（在vimrc中加入execute neocomplete#initialize()即可）
 
 * 在mac中alt的map会有问题。原因是alt加某个字符都会代笔新的字符，比如 ,在我的mac中alt-1/2/3分别是¡/™/£。所以要map alt-1，只要map ¡即可。
 
### zsh
* zsh中使用一些符号需要加上转义字符。比如rake new_post\["记录遇到的零碎问题"\],git reset HEAD\^,否则会报zsh: no matches found:错误。

