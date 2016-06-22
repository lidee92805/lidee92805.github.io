---
layout: post
title: "浏览器兼容"
date: 2016-06-22
tags: [Web]
comments: true
---

# 如何调试IE浏览器

1. 使用高版本IE的控制台

2. border: 1px solid red

3. outline: 1px solid red

4. javascript:alert(document.get...)

# 什么是CSS hack？在CSS和HTML里如何写hack？在CSS中IE6、IE7的hack方式？

CSS hack:针对不同浏览器写CSS，兼容浏览器并得到想要的效果

* HTML中hack也叫条件注释

		<!--[if IE 6]>
    	<p>You are using Internet Explorer 6.</p>
    	<![endif]-->
    	<!--[if !IE]><!-->
    	<script>alert(1);</script>
    	<!--<![endif]-->
    	<!--[if IE 8]>
    	<link href="ie8only.css" rel="stylesheet">
    	<![endif]-->
    
* CSS hack

	* 属性前缀法(即类内部Hack)：例如 IE6能识别下划线""和星号" "，IE7能识别星号" "，但不能识别下划线""，IE6~IE10都认识"\9"，但firefox前述三个都不能认识

	* 选择器前缀法(即选择器Hack)：例如 IE6能识别html .class{}，IE7能识别+html .class{}或者*:first-child+html .class{}

	* IE条件注释法(即HTML条件注释Hack)：针对所有IE(注：IE10+已经不再支持条件注释)： ，针对IE6及以下版本： 。这类Hack不仅对CSS生效，对写在判断语句里面的所有代码都会生效

	[如果记不住，猛戳这里](http://browserhacks.com)
	
	![/images/Property-hack.png]


# 列举几种浏览器兼容问题

inline-block: IE6,7不支持

		.selector{
			display:inline-block;
			*display:inline;  兼容IE6，7
		}
		
hover: IE6,7不支持，用Javascript实现

# 针对兼容、多浏览器覆盖有什么看法？渐进增强和优雅降级是什么意思？

* 优雅降级

首先确保使用现代浏览器的用户能够获得最好的用户体验，然后为使用老旧浏览器的用户优雅降级。

* 渐进增强

首先为所有浏览器都提供一个能够正常渲染并工作的版本，然后为更高级的浏览器构建高级功能。

# reset.css和normalize.css分别是做什么的？为什么推荐使用normalize.css？

reset.css是将所有的浏览器的自带样式重置掉，这样更易于保持各浏览器渲染的一致性

normalize.css是尽量保留浏览器的默认样式，不进行太多的重置

normalize对比reset的优势

1. Normalize.css 保护了有价值的默认值 Reset通过为几乎所有的元素施加默认样式，强行使得元素有相同的视觉效果。 相比之下，Normalize.css保持了许多默认的浏览器样式。 这就意味着你不用再为所有公共的排版元素重新设置样式。 当一个元素在不同的浏览器中有不同的默认值时，Normalize.css会力求让这些样式保持一致并尽可能与现代标准相符合。
2. Normalize.css 修复了浏览器的bug 它修复了常见的桌面端和移动端浏览器的bug。这往往超出了Reset所能做到的范畴。 关于这一点，Normalize.css修复的问题包含了HTML5元素的显示设置、预格式化文字的font-size问题、在IE9中SVG的溢出、许多出现在各浏览器和操作系统中的与表单相关的bug。
3. Normalize.css 不会让你的调试工具变的杂乱 使用Reset最让人困扰的地方莫过于在浏览器调试工具中大段大段的继承链，如下图所示。在Normalize.css中就不会有这样的问题，因为在我们的准则中对多选择器的使用时非常谨慎的，我们仅会有目的地对目标元素设置样式。
4. Normalize.css 是模块化的 这个项目已经被拆分为多个相关却又独立的部分，这使得你能够很容易也很清楚地知道哪些元素被设置了特定的值。因此这能让你自己选择性地移除掉某些永远不会用到部分（比如表单的一般化）。
5. Normalize.css 拥有详细的文档 Normalize.css的代码基于详细而全面的跨浏览器研究与测试。这个文件中拥有详细的代码说明并在Github Wiki中有进一步的说明。这意味着你可以找到每一行代码具体完成了什么工作、为什么要写这句代码、浏览器之间的差异，并且你可以更容易地进行自己的测试。 这个项目的目标是帮助人们了解浏览器默认是如何渲染元素的，同时也让人们很容易地明白如何改进浏览器渲染。

# IE盒模型和标准盒模型有什么区别? 怎样使 IE678使用标准盒模型?box-sizing:border-box有什么作用

ie678怪异模式（不添加doctype）使用ie盒模型，宽度=边框+padding+内容宽度

![](/images/ie-box.jpg)

chrome、ie9+、ie678（添加doctype）使用标准盒模型，宽度=内容宽度

![](/images/w3c-box.jpg)

box-sizing:border-box:设置全局的盒模型设置的宽度=边框+padding+内容宽度

本博客版权归lidee92805和饥人谷所有，转载请注明出处





