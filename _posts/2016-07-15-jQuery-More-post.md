---
layout: post
title: "jQuery"
date: 2016-07-15
tags: [Web]
comments: true
---

# jQuery中,$(document).ready()是什么意思？和window.onload的区别？还有其他什么写法或者替代方法？

* 当DOM载入就绪可以查询及操纵时绑定一个要执行的函数

	1. 执行时间
		
		* window.onload必须等到页面内包括图片的所有元素加载完毕后才能执行

		* $(document).ready()是DOM结构绘制完毕后就执行，不必等到加载完毕

	2. 编写个数不同

		* window.onload不能同时编写多个，如果有多个window.onload方法，只会执行一个

		* $(document).ready()可以同时编写多个，并且都可以得到执行

				$(document).ready(function(){})可以简化成$(function(){});

# .heml()和.text()的区别

* .html(): 取得第一个匹配元素的html内容

* .text(): 取得所有匹配元素的内容

# $.extend的作用和用法？

* 把两个或更多的对象的内容合并到第一个对象中

		var object1 = {
			name: 'lidehua',
			age: 24
		};
		var object2 = {
			name: 'lidehua',
			sex: 'man'
		}
		var object = $.extend({}, object1, object2);
		console.log(object); //Object {name: "lidehua", age: 24, sex: "man"}
		console.log(object1); //Object {name: "lidehua", age: 24}
		console.log(object2); //Object {name: "lidehua", sex: "man"}
		
		var object = $.extend(object1, object2);
		console.log(object); //Object {name: "lidehua", age: 24, sex: "man"}
		console.log(object1); //Object {name: "lidehua", age: 24, sex: "man"}
		console.log(object2); //Object {name: "lidehua", sex: "man"}
		
# JQuery的链式调用是什么？

* 并让所有定义在构造函数的prototype属性所指对象中的方法都返回用以调用方法的那个实例的引用，每个方法最后返回JQuery的对象

# JQuery Ajax中缓存怎样控制？

* .ajax() settings中的cache属性false或者true

# jQuery中data函数的作用？

* 在元素上存放数据，返回jQuery对象




本博客版权归lidee92805和饥人谷所有，转载请注明出处





