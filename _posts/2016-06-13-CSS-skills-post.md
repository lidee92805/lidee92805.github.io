---
layout: post
title: "CSS常见技巧"
date: 2016-06-13
tags: [Web]
comments: true
---

# CSS Sprite(雪碧图|精灵图)指什么？有什么作用

* CSS Sprite是将小图标和背景图像合成到一张图片上，然后利用css的背景定位来显示需要显示的图片部分。能减少加载网页图片时对服务器的请求次数，提高页面的加载速度。

# img标签和CSS背景使用图片在使用场景上有何区别

* 对于固定不变的内容，如图标等用背景图。对于可变的内容，图片和内容相关的，用img标签

# title和alt属性分别有什么作用

* alt属性用来指定替换文字，只能用在img、area和input元素中（包括applet元素），用于网页中图片无法正常显示时给用户提供文字说明使其了解图像信息。
* title可以用在任何元素上，把鼠标移动到元素上面，就会显示title的内容，以达到补充说明或者提示的效果。

	在旧版的IE浏览器中，鼠标经过图像时显示的提示文字是alt的内容，而忽略了title属性，这个曾经误导了很多人。因此，如果想在IE中显示title的内容，要么title属性和alt一致，要么alt内容为空。不过在新版的IE（IE8及以上）中，以不会出现这种情况了。
	
	另外，当a标签内嵌套img标签时，起作用的是img的title属性。
	
# background: url(abc.png) 0 0 no-repeat;这句话是什么意思

设置背景为当前目录下的abc图片，x、y偏移为0，图片显示不重复

# background-size有什么作用？兼容性如何？常用的值是？

规定背景图像的尺寸

![](/images/background-size-compatible.png)

* length: 设置背景图像的高度和宽度。第一个值为宽度，第二个值为高度。如果只设置一个值，则第二个值会被设置为auto
* percentage: 以父元素的百分比来设置背景图像的宽度和高度。第一个值设置宽度，第二个值设置高度。如果只设置一个值，则第二个值会被设置为auto
* cover: 把背景图像扩展至足够大，以使背景图像完全覆盖背景区域。背景图像的某些部分也许无法显示在背景定位区域中
* contain: 把图像扩展至最大尺寸，以使其宽度和高度完全适应内容区域

# 如何让一个div水平居中？如何让图片水平居中

div水平居中: margin: 0 auto;

图片水平居中: text-align: center;

# 如何设置元素透明？兼容性？

	img {
		opacity: 0;
		filter: alpha(opacity=0);For IE8 and earlier
	}
	
![](/images/opacity-compatible.png)

# opacity和rgba都能设置透明，有什么区别

opacity会继承父元素的opacity属性，而RGBA设置的元素的后代元素不会继承不透明属性。

本博客版权归lidee92805和饥人谷所有，转载请注明出处





