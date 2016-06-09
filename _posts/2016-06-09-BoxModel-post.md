---
layout: post
title: "CSS盒模型"
date: 2016-06-09
tags: [Web]
comments: true
---

# 盒模型包括哪些属性

* element:元素
* padding:内边距
* border:边框
* margin:外边距

![](/images/css-box.png)

# text-align:center的作用是什么，作用在什么元素上？能让什么元素水平居中？

text-align: center作用在块级元素上，能让块级元素内的行内元素水平居中

# 如果遇到一个属性想知道兼容性，在哪查看

在[Can I use](http://caniuse.com)网站查看

# IE盒模型盒W3C盒模型有什么区别？
ie678怪异模式（不添加doctype）使用ie盒模型，宽度=边框+padding+内容宽度

![](/images/ie-box.jpg)

chrome、ie9+、ie678（添加doctype）使用标准盒模型，宽度=内容宽度

![](/images/w3c-box.jpg)

# 以下代码的作用？兼容性？

	* {
		box-sizing: border-box;
	}
	
设置全局的盒模型设置的宽度=边框+padding+内容宽度

![](/images/box-sizing.png)


本博客版权归lidee92805和饥人谷所有，转载请注明出处





