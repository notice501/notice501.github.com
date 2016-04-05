---
layout: post
title: "web app指南之构建html5离线应用"
date: 2013-02-25 13:02
comments: true
categories: [html5]
---

创建运行在手机上的web app时，鉴于手机用户的网络情况，我们需要考虑到用户离线使用的情况。

html5支持构建离线应用程序。使用它的本地缓存机制可以将应用所需的资源文件都缓存到本地，从而实现应用的离线使用。首先要说明的是，本地缓存和传动的浏览器网页缓存是不同的，网页缓存基于网页，也就是缓存一个网页的内容，而不是整个app。同时网页缓存并不可靠，我们不知道我们的app中哪个页面已经缓存，该页面的哪些资源已经缓存，而本地缓存对于缓存内容是完全可控的。

<!-- more -->
使用离线缓存，除了可以使应用可以离线使用外，还能帮助有效的加快网页加载速度（本地的自然更快），同时降低服务器负载（只需要下载更新的内容）。

正如之前所提到的，本地缓存可以指定要缓存的内容，这同过配置manifest来实现。可以为整个app配置manifest，也可以为单独某个页面来配置。

简单的manifest格式如下：
{% codeblock lang:html %}
CACHE MANIFEST
index.html
stylesheet.css
images/logo.png
scripts/main.js
{% endcodeblock %}

文件的第一行必须是CACHE MANIFEST。

该manifest声明了需要缓存的html页面，css，图片以及js文件。

再看一个比较复杂的manifest文件：

{% codeblock lang:html %}
CACHE MANIFEST
# 指定一个版本号
# version 1
# 该类别指定要缓存的资源文件
CACHE:
/favicon.ico
index.html
stylesheet.css
images/logo.png
scripts/main.js

# 指定不进行缓存的资源文件
NETWORK:
login.php
http://foocoder.com

# 每行指定两个文件，第一个为在线时使用的资源，第二个是离线时使用的资源
FALLBACK:
/main.py /static.html
images/large/ images/offline.jpg
*.html /offline.html
{% endcodeblock %}

其中#号开头的为注释。因为只有在manifest文件发生改变时才会更新，所以我们可以加个版本号方便控制。

从该文件中可以看到分为了三个类别：

CACHE类别指定需要被缓存的资源文件。

NETWORK类别指定不缓存的资源文件，即只在联网的情况下才能访问。

FALLBACK每一行都会指定两个文件，第一个为在线时使用的资源，第二个为离线时使用的备用资源。其中*为通配符，表示在线时使用所有的.html文件。

配置好manifest文件之后，我们只需要在页面上引用即可。如下，在html 标签的manifest属性下指定manifest文件的地址：

{% codeblock lang:html %}
<html manifest="app.manifest">
...
</html>
{% endcodeblock %}

该地址可以是绝对地址也可以是相对地址，但是该文件的吗MIME 类型必须是text/cache-manifest，所以需要在服务器做相应配置对该类型添加支持，例如吗，对于apache服务器，需要在配置mime.types中添加如下内容：

AddType text/cache-manifest .manifest
到这里为止，就完成了离线缓存的基本内容，在manifest文件发生变化时，浏览器会检查manifest文件并更新缓存。

我们不得不考虑一个问题，浏览器如何处理本地缓存？当服务端更新了应用程序后，用户打开时是不是会使用最新的资源了？答案是否定的。这需要了解下在使用离线缓存的情况下，浏览器与服务端的整个交互过程。

1.首次访问

在首次访问时，没有什么特别，浏览器解析index.html，请求所有的资源文件。随后就会处理manifest文件，请求所有的manifest中的资源文件，注意，即使之前已经请求过了所有的资源文件，这里必须进行重复请求。最后将这些文件缓存到本地。

2.再次访问

再次访问时，浏览器发现有本地缓存，所以会加载本地缓存内容。随后会向服务端请求manifest文件，如果manifest文件未更新，返回304代码，浏览器不做处理。如果manifest已经更新过，则请求所有manifest中的资源文件，重新对其缓存。

所以，即使服务端更新了manifest和其他资源，用户打开时扔是之前的页面。需要重新打开才能使用更新过后的资源。

有办法立刻更新缓存么？是可以的。我们可以使用applicationCache对象做到这一点。但是也只是能做到立刻更新缓存，还是需要用户重新打开也没才会生效。接下来就看看如何用applicationCache对象立刻更新缓存。

window.applicationCache下有个status属性。可以通过其知道当前的缓存状态

{% codeblock lang:js %}
var appCache = window.applicationCache; 
  
switch (appCache.status) { 
  case appCache.UNCACHED: // UNCACHED == 0 
    return 'UNCACHED'; 
    break; 
  case appCache.IDLE: // IDLE == 1 
    return 'IDLE'; 
    break; 
  case appCache.CHECKING: // CHECKING == 2 
    return 'CHECKING'; 
    break; 
  case appCache.DOWNLOADING: // DOWNLOADING == 3 
    return 'DOWNLOADING'; 
    break; 
  case appCache.UPDATEREADY:  // UPDATEREADY == 4 
    return 'UPDATEREADY'; 
    break; 
  case appCache.OBSOLETE: // OBSOLETE == 5 
    return 'OBSOLETE'; 
    break; 
  default: 
    return 'UKNOWN CACHE STATUS'; 
    break; 
};
{% endcodeblock %}

既然可以获得状态，我们只需要请求更新，随后在状态为appCache.UPDATEREADY时更新缓存时即可。

{% codeblock lang:js %}
applicationCache.update()方法会尝试更新用户缓存，而applicationCache.swapCache()方法会对本地缓存进行更新：

var appCache = window.applicationCache;
 
appCache.update(); // 开始更新
 
if (appCache.status == window.applicationCache.UPDATEREADY) {
  appCache.swapCache();  // 更新缓存
}
{% endcodeblock %}

正如之前所说的，即使更新了缓存，还是需要重新加载才能使用最新的资源，此时可以提示用户更新。只需要监听onUpdateReady事件，该事件在缓存被下载到本地后出发，从而可以在此时提示用户：

{% codeblock lang:js %}
window.addEventListener('load', function(e) {

  window.applicationCache.addEventListener('updateready', function(e) {
    if (window.applicationCache.status == window.applicationCache.UPDATEREADY) {
     //更新本地缓存
      window.applicationCache.swapCache();
      if (confirm('已经有新的版本，是否立刻切换到最新版?')) {
        window.location.reload();
      }
    } else {
     
    }
  }, false);

}, false);
{% endcodeblock %}

applicationCache对象还提供了其他事件，分别为：

onchecking，onerror，onnoupdate，ondownloading，onprogress，onupdateready，oncached和onobsolete

在整个浏览器与服务端交互的过程中，所有的错误都会出发error事件，我们可以通过监听error事件进行处理：

{% codeblock lang:js %}
var appCache = window.applicationCache;
 
appCache.addEventListener('error', handleCacheError, false);
 
function handleCacheError(e) {
  alert('Error: Cache failed to update!');
};
{% endcodeblock %}

欢迎留言交流。
