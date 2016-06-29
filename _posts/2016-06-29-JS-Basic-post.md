---
layout: post
title: "JavaScript基础"
date: 2016-06-29
tags: [Web]
comments: true
---

# CSS和JS在网页中的放置顺序是怎样的？

		<!DOCTYPE html>
		<html>
		<head>
			<meta charset="utf-8">
			<title></title>
			<style>
				//  CSS
			</style>
		</head>
		<body>
		   //html code
			<script>
				//JS
			</script>
		</body>
		</html>
		
# 解释白屏和FOUC

## 白屏

如果把样式放在底部，对于IE浏览器，在某些场景下（新窗口打开，刷新等）页面会出现白屏，而不是内容逐步展现

如果使用@import标签，即使CSS放入link，并且放在头部，也可能出现白屏。

对于图片和CSS，在加载时会并发加载（如一个域名下同时加载两个文件）。但在加载JavaScript时，会禁用并发，并且阻止其他内容的下载，所以把JavaScript放入页面顶部也会导致白屏现象

## FOUC(Flash of Unstyled Content)无样式内容闪烁

如果把样式放在底部，对于IE浏览器，在某些场景下（点击链接，输入URL，使用书签进入等），会出现FOUC现象（逐步加载无样式内容，等CSS加载后页面突然展现样式）。对于Firefox会一直表现出FOUC

# async和defer的作用是什么？有什么区别

async:
		
		<script async src="script.js"></script>
		
		加载和渲染后续文档元素的过程将和script.js的加载与执行并行进行（异步）

defer:

		<script defer scr="script.js"></script>
		
		加载后续文档元素的过程将和script.js的加载并行进行（异步），但是script.js的执行要在所有元素解析完成之后，DOMContentLoaded事件触发之前完成
		
区别：脚本下载完之后何时执行，async加载完了会立刻执行，defer在所有元素解析完成后，DOMContentLoaded事件触发之前执行

# 简述网页的渲染机制

* 解析HTML标签，构建DOM树

* 解析CSS标签，构建CSSOM树

* 把DOM和CSSOM组合成渲染树

* 在渲染树的基础上进行布局，计算每个节点的几何结构

* 把每个节点绘制到屏幕上

# JavaScript定义了几种数据类型？哪些是简单类型？哪些是复杂类型

1. Null

	Null类型只有一个值：null，表示空指针，也就是不存在的东西
	
2. Undefined

	Undefined类型也只有一个值undefined，表示变量只被声明，没有被初始化，也就是有这个指针，但是这个指针没有指向任何空间
	
3. Boolean

	Boolean有两个值，true和false
	
4. Number

	JavaScript的数字类型和其它语言有所不同，没有整型和浮点数的区别，统一都是Number类型，可以表示十进制、八进制、十六进制
	
5. String
	
	String是Unicode字符组成的序列，俗称字符串，可以用双引号或者单引号表示，没有区别，匹配即可
	
6. Object

	一种无序的数据集合，由若干个“键值对”（key-value）构成。key我们称为对象的属性，value可以是任何JavaScript类型，甚至可以是对象
	
# NaN、undefined、null分别代表什么

* NaN含义是Not a Number，表示非数字，NaN和任何值都不相等，包括自己

* Undefined类型也只有一个值undefined，表示变量只被声明，没有被初始化，也就是有这个指针，但是这个指针没有指向任何空间

* null，表示空指针，也就是不存在的东西

# typeof和instanceof的作用和区别？

* typeof返回一个表达式的数据类型的字符串，返回结果为js基本的数据类型，包括number,boolean,string,object,undefined,function.

* instanceof则为判断一个对象是否为某一数据类型，或一个变量是否为一个对象的实例;返回boolean类型

* 我们可以使用typeof来获取一个变量是否存在，如if(typeof a!="undefined"){}，而不要去使用if(a)因为如果a不存在（未声明）则会出错，对于Array,Null等特殊对象使用typeof 一律返回object，这正是typeof的局限性。

本博客版权归lidee92805和饥人谷所有，转载请注明出处





