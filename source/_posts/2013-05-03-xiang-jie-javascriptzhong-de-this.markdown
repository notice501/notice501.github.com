---
layout: post
title: "详解JavaScript中的this"
date: 2013-05-03 19:28
comments: true
categories: [JavaScript]
---
JavaScript中的this总是让人迷惑，应该是js众所周知的坑之一。
个人也觉得js中的this不是一个好的设计，由于this晚绑定的特性，它可以是全局对象，当前对象，或者…有人甚至因为坑大而不用this。

其实如果完全掌握了this的工作原理，自然就不会走进这些坑。来看下以下这些情况中的this分别会指向什么：
<!--more-->

###1.全局代码中的this


``` javascript
alert(this)//window
```

全局范围内的this将会指向全局对象，在浏览器中即使window。

###2.作为单纯的函数调用

``` javascript
function fooCoder(x) {
	this.x = x;
}
fooCoder(2);
alert(x);// 全局变量x值为2
```

这里this指向了全局对象，即window。在严格模式中，则是undefined。

###3.作为对象的方法调用

``` javascript
var name = "clever coder";
var person = {
	name : "foocoder",
	hello : function(sth){
		console.log(this.name + " says " + sth);
	}
}
person.hello("hello world");
```

输出 foocoder says hello world。this指向person对象，即当前对象。


###4.作为构造函数

``` javascript
new FooCoder();
```

函数内部的this指向新创建的对象。

###5.内部函数

``` javascript
var name = "clever coder";
var person = {
	name : "foocoder",
	hello : function(sth){
		var sayhello = function(sth) {
			console.log(this.name + " says " + sth);
		};
		sayhello(sth);
	}
}
person.hello("hello world");//clever coder says hello world
```

在内部函数中，this没有按预想的绑定到外层函数对象上，而是绑定到了全局对象。这里普遍被认为是JavaScript语言的设计错误，因为没有人想让内部函数中的this指向全局对象。一般的处理方式是将this作为变量保存下来，一般约定为that或者self：

``` javascript
var name = "clever coder";
var person = {
	name : "foocoder",
	hello : function(sth){
		var that = this;
		var sayhello = function(sth) {
			console.log(that.name + " says " + sth);
		};
		sayhello(sth);
	}
}
person.hello("hello world");//foocoder says hello world
```

###6.使用call和apply设置this
``` javascript
person.hello.call(person, "world");
```

apply和call类似，只是后面的参数是通过一个数组传入，而不是分开传入。两者的方法定义：

	call( thisArg [，arg1，arg2，… ] );  // 参数列表，arg1，arg2，...
	apply(thisArg [，argArray] );     // 参数数组，argArray

两者都是将某个函数绑定到某个具体对象上使用，自然此时的this会被显式的设置为第一个参数。

##简单地总结

简单地总结以上几点，可以发现，其实只有第六点是让人疑惑的。

其实就可以总结为以下几点：

1.当函数作为对象的方法调用时，this指向该对象。

2.当函数作为淡出函数调用时，this指向全局对象（严格模式时，为undefined）

3.构造函数中的this指向新创建的对象

4.嵌套函数中的this不会继承上层函数的this，如果需要，可以用一个变量保存上层函数的this。

再总结的简单点，如果在函数中使用了this，只有在该函数直接被某对象调用时，该this才指向该对象。

``` javascript	
	obj.foocoder();
	foocoder.call(obj, ...);
	foocoder.apply(obj, …);
```
	
##更进一步

我们可能经常会写这样的代码：

``` javascript
$("#some-ele").click = obj.handler;
```

如果在handler中用了this，this会绑定在obj上么？显然不是，赋值以后，函数是在回调中执行的，this会绑定到$("#some-div")元素上。这就需要理解函数的执行环境。本文不打算长篇赘述函数的执行环境，可以参考《javascript高级程序设计》中对执行环境和作用域链的相关介绍。这里要指出的时，理解js函数的执行环境，会更好地理解this。

那我们如何能解决回调函数绑定的问题？ES5中引入了一个新的方法，bind():

	fun.bind(thisArg[, arg1[, arg2[, ...]]])
	
	thisArg
	当绑定函数被调用时,该参数会作为原函数运行时的this指向.当使用new 操作符调用绑定函数时,该参数无效.
	arg1, arg2, ...
	当绑定函数被调用时,这些参数加上绑定函数本身的参数会按照顺序作为原函数运行时的参数.
	
该方法创建一个心函数，称为绑定函数，绑定函数会以创建它时传入bind方法的第一个参数作为this，传入bind方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数.

显然bind方法可以很好地解决上述问题。

``` javascript
$("#some-ele").click(person.hello.bind(person));
//相应元素被点击时，输出foocoder says hello world
```

其实该方法也很容易模拟，我们看下Prototype.js中bind方法的源码：

``` javascript
Function.prototype.bind = function(){ 
  var fn = this, args = Array.prototype.slice.call(arguments), object = args.shift(); 
  return function(){ 
    return fn.apply(object, 
      args.concat(Array.prototype.slice.call(arguments))); 
  }; 
};
```

明白了么？

相信看完全文以后，this不再是坑～
