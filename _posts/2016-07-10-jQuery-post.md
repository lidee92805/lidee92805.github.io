---
layout: post
title: "jQuery"
date: 2016-07-10
tags: [Web]
comments: true
---

# 说说库和框架的区别

* 库是将代码集合成的一个产品，供程序员调用

* 框架则是为解决一个(一类)问题而开发的产品，框架用户一般只需要使用框架提供的类或函数，即可实现全部功能

# jQuery能做什么？

jQuery是一个快，小并且功能丰富的JavaScript库。它是跨浏览器，对DOM元素操作，事件处理，动画和Ajax提供了容易使用的API。

# jQuery对象和DOM原生对象有什么区别？如何转化？

* jQuery对象只能使用jQuey对象提供的API，DOM原生对象只能使用DOM原生对象提供的API

* jQuery对象＝$(DOM对象) 

* DOM对象＝jQuery对象[0]

# jQuery中如何绑定事件？bind、unbind、delegate、live、on、off都有什么作用？推荐使用哪种？使用on绑定事件使用事件代理的写法？

		$('#foo').bind('click',function(){});
		
* bind: 给元素绑定事件

* unbind: 移除之前绑定的事件

* delegate: 根据选择器匹配根元素下的所有元素绑定一个或多个事件

* live: 匹配当前选择器的所有元素绑定一个或多个事件

* on: 选中的元素绑定一个或多个事件

* off: 移除一个事件绑定

		$( selector ).live( events, data, handler );                // jQuery 1.3+
		$( document ).delegate( selector, events, data, handler );  // jQuery 1.4.3+
		$( document ).on( events, selector, data, handler );        // jQuery 1.7+

# jquery 如何展示/隐藏元素？

* .hide([duration ] [,easing ] [,complete ])

	用于隐藏元素，没有参数的时候等同于直接设置display属性
	
* .show( [duration ] [, easing ] [, complete ] )

	用于显示元素，用法和hide类似
	
# jquery 动画如何使用？

* animate( properties [, duration ] [, easing ] [, complete ] )

* properties是一个CSS属性和值的对象,动画将根据这组对象移动

* duration (default: 400)：一个字符串或者数字决定动画将运行多久。默认值: "normal"， 三种预定速度的字符串("slow", "normal", 或 "fast"或表示动画时长的毫秒数值(如：1000) ）

* easing (default: swing)：一个字符串，表示过渡使用哪种缓动函数。jQuery自身提供"linear" 和 "swing"，其他效果可以使用jQuery Easing Plugin插件

* complete：在动画完成时执行的函数

# 如何设置和获取元素内部 HTML 内容？如何设置和获取元素内部文本？

* HTML内容   html()方法，带参数为设置，不带参数获取

* 文本内容  text()方法，带参数为设置，不带参数获取

# 如何设置和获取表单用户输入或者选择的内容？如何设置和获取元素属性？

* 表单内容  val()方法，带参数为设置，不带参数获取

* 元素属性  attr()方法，带参数为设置，不带参数获取

本博客版权归lidee92805和饥人谷所有，转载请注明出处





