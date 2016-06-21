---
layout: post
title: "边距合并和BFC"
date: 2016-06-21
tags: [Web]
comments: true
---

# 在什么场景下会出现外边距合并？如何合并？如何不让相邻元素外边距合并？给个父子外边距合并的范例

外边距合并的必备条件：margin必须是邻接的

两个margin是邻接的必需满足以下条件：

* 必须是处于常规文档流（非float和绝对定位）的块级盒子，并且处于同一个BFC当中

* 没有线盒，没有空隙，没有padding和border将他们分隔开

* 都属于垂直方向上相邻的外边距，可以是下面任意一种情况

	* 元素的margin-top与其第一个常规文档流的子元素的margin-top

	* 元素的margin-bottom与其下一个常规文档流的兄弟元素的margin-top

	* height为auto的元素的margin-bottom与其最后一个常规文档流的子元素的margin-bottom

	* 高度为0并且最小高度也为0，不包含常规文档流的子元素，并且自身没有建立斤的BFC的元素的margin-top和margin-bottom

合并规则：

* 一个常规文档流元素的margin-bottom与它下一个常规文档流的兄弟元素的margin-top会合并，除非它们之间存在clearance

* 一个常规文档流元素的margin-top与其第一个常规文档流的子元素的margin-top产生合并，条件为父元素不包含padding和border,子元素不包含clearance

* 一个'height'为'auto'并且'min-height'为0的常规文档流元素的margin-bottom会与其最后一个常规文档流子元素的margin-bottom合并，条件为父元素不包含padding和border,子元素的margin-bottom不与保护clearance的margin-top合并

* 一个不包含boder-top、border-bottom、padding-top、padding-bottom的常规文档流元素，并且其'height'为0或'auto','min-height'为'0',其里面也不包含line box,其自身的margin-top和margin-bottom会合并

* 创建了新的BFC的元素（例如浮动元素或者'overflow'值为'visible'以外的元素）与它的子元素的外边距不会合并

* 浮动元素不与任何元素的外边距产生合并（包括其父元素和子元素）

* 绝对定位元素不与任何元素的外边距产生合并

* inline-block元素不与任何元素的外边距产生合并

![](/images/margin-merge.png)

# 去除inline-block内缝隙有哪几种常见方法

* 去除行内元素之间的空格和换行

		<div class="inline-f00">inline</div><div class="inline-0f0">inline</div><div class="inline-block-00f">inline-block</div><div class="inline-block-000">inline-block</div>
		
或者使用注释分隔
		
		<div class="inline-f00">inline</div><!--
		--><div class="inline-0f0">inline</div><!--
		--><div class="inline-block-00f">inline-block</div><!--
		--><div class="inline-block-000">inline-block</div>
		
还可以

		<div class="inline-f00">inline</div
		><div class="inline-0f0">inline</div
		><div class="inline-block-00f">inline-block</div
		><div class="inline-block-000">inline-block</div>
		
* 设置父元素的font-size:0,子元素设置字体大小为正常姿态大小

		.parent {
			font-size: 0px;
		}
		.child {
			font-size: 16px;
		}
		
* 行内元素设置为浮动元素,行内元素之间的间距会消失

	![](/images/float-to-clear-clearance)
	
* 设置行内元素margin-left为-4px,调整第一个元素的位置

# 父容器使用overflow: auto | hidden撑开高度的原理是什么

overflow: auto | hidden会形成BFC，可以包裹浮动流，全部浮动子元素也不会引起容器高度塌陷，包含块会把浮动元素的高度也计算在内

# BFC是什么？如何形成BFC，有什么作用

浮动元素和绝对定位元素，非块级盒子的块级容器（例如 inline-blocks, table-cells, 和 table-captions），以及overflow值不为“visiable”的块级盒子，都会为他们的内容创建新的BFC（块级格式上下文）。

在BFC中，盒子从顶端开始垂直地一个接一个地排列，两个盒子之间的垂直的间隙是由他们的margin 值所决定的。在一个BFC中，两个相邻的块级盒子的垂直外边距会产生折叠。

在BFC中，每一个盒子的左外边缘（margin-left）会触碰到容器的左边缘(border-left)（对于从右到左的格式来说，则触碰到右边缘）。

BFC出发条件：

* 浮动元素（float除了none）

* 绝对定位元素（absolute/fixed）

* 设置了display属性为inline-block,table-cell,table-caption的元素

* 设置了overflow非visible的元素

BFC的作用

* 不和浮动元素重叠

* 清除元素内部浮动

# 浮动导致的父容器高度塌陷指什么？为什么会产生？有几种解决办法

当父元素至包含浮动的元素的时候且父元素没有设置高度，父容器不会把浮动子元素的高度计算在内

产生原因：计算容器元素高度的时候，浮动元素相当于脱离了文档流

解决办法：

* 清除浮动 clear: both | left | right

* 触发BFC撑开高度

# 分析代码

	.clearfix:after {
		content: '';  在clearfix元素最后面加入内容
		display: block; 默认是inline，转换为block
		clear: both; 清除浮动
	}
	.clearfix {
		*zoom: 1;  触发 hasLayout  这是针对于IE6的，因为IE6不支持:after伪类，这个神奇的zoom:1让IE6的元素可以清除浮动来包裹内部元素
	}

清除浮动元素造成的影响而不是像BFC撑开容器的高度


本博客版权归lidee92805和饥人谷所有，转载请注明出处





