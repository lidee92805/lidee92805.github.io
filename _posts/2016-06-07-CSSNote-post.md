---
layout: post
title: "CSS选择器"
date: 2016-06-07
tags: [Web]
comments: true
---

# CSS选择器常见的有几种？

## 基础选择器
 
选择器 | 含义
------- | -------
＊ | 通用元素选择器，匹配页面任何元素（这也就决定了我们很少使用）
 #id | id选择器，匹配特定id的元素
.class | 类选择器，匹配class包含(不是等于)特定类的元素
element | 标签选择器

## 组合选择器

选择器 | 含义
------- | -------
E,F | 多元素选择器，用,分隔，同时匹配元素E或元素F
 E F | 后代选择器，用空格分隔，匹配E元素所有的后代（不只是子元素、子元素向下递归）元素F
E>F | 子元素选择器，用>分隔，匹配E元素的所有直接子元素
E+F | 直接相邻选择器，匹配E元素之后的相邻的同级元素F
E~F | 普通相邻选择器（弟弟选择器），匹配E元素之后的同级元素F（无论直接相邻与否）
.class1.class2 | id和class选择器和选择器连写的时候中间没有分隔符，. 和 # 本身充当分隔符的元素
element#id | id和class选择器和选择器连写的时候中间没有分隔符，. 和 # 本身充当分隔符的元素

## 属性选择器

选择器 | 含义
------- | -------
E[attr] | 匹配所有具有属性attr的元素，div[id]就能取到所有有id属性的div
 E[attr = value] | 匹配属性attr值为value的元素，div[id=test],匹配id=test的div
E[attr ~= value] | 匹配所有属性attr具有多个空格分隔、其中一个值等于value的元素
E[attr ^= value] | 匹配属性attr的值以value开头的元素
EE[attr $= value] | 匹配属性attr的值以value结尾的元素
E[attr *= value] | 匹配属性attr的值包含value的元素

## 伪类选择器

选择器 | 含义
------- | -------
E:first-child | 匹配元素E的第一个子元素
 E:link | 匹配所有未被点击的链接
E:visited | 匹配所有已被点击的链接
E:active | 匹配鼠标已经其上按下、还没有释放的E元素
E:hover | 匹配鼠标悬停其上的E元素
E:focus | 匹配获得当前焦点的E元素
E:lang(c) | 匹配lang属性等于c的E元素
E:enabled | 匹配表单中可用的元素
E:disabled | 匹配表单中禁用的元素
E:checked | 匹配表单中被选中的radio或checkbox元素
E:selection | 匹配用户当前选中的元素
E:root | 匹配文档的根元素，对于HTML文档，就是HTML元素
E:nth-child(n) | 匹配其父元素的第n个子元素，第一个编号为1
nth-last-child(n) | 匹配其父元素的倒数第n个子元素，第一个编号为1
E:nth-of-type(n) | 与:nth-child()作用类似，但是仅匹配使用同种标签的元素
E:nth-last-of-type(n) | 与:nth-last-child() 作用类似，但是仅匹配使用同种标签的元素
E:last-child | 匹配父元素的最后一个子元素，等同于:nth-last-child(1)
E:first-of-type | 匹配父元素下使用同种标签的第一个子元素，等同于:nth-of-type(1)
E:last-of-type | 匹配父元素下使用同种标签的最后一个子元素，等同于:nth-last-of-type(1)
E:only-child | 匹配父元素下仅有的一个子元素，等同于:first-child:last-child或 :nth-child(1):nth-last-child(1)
E:only-of-type | 匹配父元素下使用同种标签的唯一一个子元素，等同于:first-of-type:last-of-type或 :nth-of-type(1):nth-last-of-type(1)
E:empty | 匹配一个不包含任何子元素的元素，文本节点也被看作子元素
E:not(selector) | 匹配不符合当前选择器的任何元素

## 伪元素选择器

选择器 | 含义
------- | -------
E:first-line | 匹配E元素内容的第一行
 E:first-letter | 匹配E元素内容的第一个字母
E:before | 在E元素之前插入生成的内容
E:after | 在E元素之后插入生成的内容

		
# 选择器的优先级是怎样的？

从高到低分别是

1. 在属性后面使用!important会覆盖页面内任何位置定义的元素样式

2. 作为style属性写在元素标签上的内联样式

3. id选择器

4. 类选择器

5. 伪类选择器

6. 属性选择器

7. 标签选择器

8. 通配符选择器

9. 浏览器自定义


# class和id的使用场景？
页面大的模组，用id来区分模组；用js的GetElementByID函数时用id，其他情况用class

# 使用CSS选择器时为什么要规定适当的命名空间？

能够简单快速的了解各个函数、模块的作用

# 分析代码

	#header {
		//匹配id为header的标签
	}
	.header {
		//匹配class为header的标签
	}
	.header .logo {
		//匹配class为header标签下class为logo的标签
	}
	.header.mobile {
		//匹配class为header和mobile的标签
	}
	.header p, .header h3 {
		//匹配class为header标签下p标签或class为header标签下h3标签
	}
	#header .nav>li {
		//匹配id为header标签下class为nav的直接子元素的li标签
	}
	#header a:hover {
		//匹配id为header标签下a标签的鼠标悬停其上的标签
	}
	
# :first-child和:first-of-type的作用和区别
:first-child匹配的是某父元素的第一个子元素，先取第一个子元素然后再匹配
:first-of-type匹配的是某父元素下相同类型子元素中的第一个，先匹配然后取第一个子元素

# 运行如下代码，解析输出样式的原因

	<style>
		.item1:first-child {
			color: red;
		}
		.item:first-of-type {
			background: blue;
		}
	</style>
	<div class="ct">
		<p class="item1">aa</p>
		<h3 class="item1">bb</h3>
		<h3 class="item1">ccc</h3>
	</div>
	
![](/images/CSS选择器.png)

.item:first-child匹配的是父元素下第一个class为item1的标签，所以aa为红色
.item:first-of-type匹配的是父元素下，相同标签的第一个元素，aa为p标签的第一个元素，bb是h3标签的第一个元素，所以都为蓝色

# text-align: center的作用是什么，作用在什么元素上？能让什么元素水平居中
text-align: center作用在块级元素上，能让块级元素内的行内元素水平居中

# 如果遇到一个属性想知道兼容性，在哪查看
在[Can I use](http://caniuse.com)网站查看


本博客版权归lidee92805和饥人谷所有，转载请注明出处


