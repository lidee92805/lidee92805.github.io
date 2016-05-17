---
layout: post
title: "html基础笔记"
date: 2016-05-17
tags: [Web]
comments: true
---
# 样式有几种引入方式？link和@import有什么区别
	
* 样式有三种引入方式
	
外部样式表
 
	<link rel="stylesheet" type="text/css" href="index.css">
 
内部样式表
   	
	<style type="text/css">
	p {
	color: red;
	}
	</style>
    
内联样式
   
	<p style="color:blue; font-size:14px;">段落</p>
	
    
1. link属于XHTML标签，除了加载CSS还可以定义RSS，rel连接属性等，@import是CSS提供的一种方式，只能加载CSS
2. 当一个页面被加载的时候，link引用的CSS会同时被加载，而@import引用的CSS会等到页面全部被下载完再被加载
3. 由于@import是CSS2.1提出的所以老的浏览器不支持，只有在IE5以上的才能识别，而link标签无此问题
4. 当使用javascript控制dom去改变样式的时候，只能使用link标签，因为@import不是dom可以控制的
5. @import可以在CSS中再次引入其他的样式表
	
# 文件路径../main.css、./main.css、main.css有什么区别
	
* ../main.css是访问当前目录的上级目录下的main.css文件
./main.css和main.css都是访问当前目录下的main.css文件
    
# console.log是做什么用的
	
* 输出信息到web控制台
	
# text-align有几个值，分别有什么作用
	
* left  文字靠左对齐
right   文字靠右对齐
center  文字居中
justify 两端对齐
inherit 继承父类属性的值
    
# px、em、rem分别是什么？有什么区别？如何使用
	
* px是像素值，rem和em是由浏览器基于设计中的字体大小计算得到的像素值
* em基于使用他们的元素的字体大小
* rem基于html元素的字体大小
* em可能受任何继承的父元素字体大小影响
* rem可以从浏览器字体设置中继承字体大小
* 使用em应根据组件的字体大小而不是根元素的字体大小
* 在不需要使用em，并且需要根据浏览器的字体大小设置缩放的情况下使用rem
* 使用rem单位，除非确定需要em单位，包括对字体大小
* 媒体查询中使用rem单位
* 不要在多列布局中使用em或rem改用％
	
# 对chrome审查元素的功能做个简单的截图介绍
	
	
![](/images/task5shoot.png)
	
	
	
	


