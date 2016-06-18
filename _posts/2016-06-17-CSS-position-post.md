---
layout: post
title: "CSS定位和浮动"
date: 2016-06-17
tags: [Web]
comments: true
---

# 文档流的概念指什么？有哪种方式可以让元素脱离文档流？

* 文档流指的是元素按照其在HTML中的位置顺序决定排布的过程，或者说在排布过程中将窗体自上而下分成一行行，并在每行中按从左至右的顺序排放元素。

	float，position: absolute，position: fixed都会使元素脱离文档流。
	
# 有几种定位方式，分别是如何实现定位的，使用场景如何？

值 | 属性
----- | -----
inherit | 规定应该从父元素继承position属性的值
static | 默认值，没有定位，元素出现在正常的流中(忽略top，bottom，left，right或者z-index声明)
relative | 	生成相对定位的元素，相对于元素本省正常位置进行定位，因此，left: 20px会向元素的left位置添加20px
absolute | 生成绝对定位的元素，相对于static定位以外的第一个祖先元素(offset parent)进行定位，元素的位置通过left，top，right以及bottom属性进行规定
fixed | 生成绝对定位的元素，相对于浏览器窗口进行定位。元素的位置通过left，top，right以及bottom属性进行规定
sticky | CSS3新属性，表现类似position: relative和position: fixed的合体，在目标区域在屏幕中可见时，它的行为就像position: relative；而当页面滚动超出目标区域时，它的表现就像position: fixed，它会固定在目标位置

# absolute,relative,fixed偏移的参考点分别是什么

absolute相对于static定位以外的第一个祖先元素
relative相对于元素本身正常位置进行定位
fixed相对于浏览器窗口进行对味

# z-index有什么作用？如何使用？

z-index属性决定了一个HTML元素的层叠级别。z-index值越大，这个元素在层叠顺序中更靠近顶部。

# position: relative和负margin都可以使元素发生偏移，二者有什么区别？

margin是外间距，是以相对另外一个元素的距离为单位，会影响其他元素的布局

position不会，在不脱离文档流情况下，不会影响其他元素的布局

![](/images/margin-relative.png)

# 如何让一个固定宽高的元素在页面上垂直水平居中？

![](/images/VHCenter.png)

# 浮动元素有什么特征？对其他浮动元素、普通元素、文字分别有什么影响？

* 浮动元素会脱离正常的文档流，按照其外边距指定的位置相对于它的上一个块级元素(或父元素)显示

* 浮动元素后面的块级元素的内容会向此浮动元素的外边距靠齐，但是边框和背景却忽略浮动元素而向上一个任意非浮动元素靠齐

* 浮动元素后面的内联元素会向此浮动元素的外边距靠齐

# 清除浮动指什么？如何清除浮动

清除浮动指清除掉元素的float属性

clear: left清除左浮动
clear: right清除右浮动
clear: both清除两侧浮动


本博客版权归lidee92805和饥人谷所有，转载请注明出处





