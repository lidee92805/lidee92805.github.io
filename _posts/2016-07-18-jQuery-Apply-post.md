---
layout: post
title: "jQuery应用"
date: 2016-07-18
tags: [Web]
comments: true
---

# 如何判断一个元素是否出现在窗口可视范围(浏览器的上边缘和下边缘之间，肉眼可视)。

		function isVisible($node) {
			var windowWidth = $(window).width();
			var windowHeight = $(window).height();
			var topOffset = $(window).scrollTop();
			var leftOffset = $(window).scrollLeft();
			var nodeTop = $node.offset().top;
			var nodeLeft = $node.offset().left;
			var nodeWidth = $node.width();
			var nodeHeight = $node.height();
			if ((nodeTop > topOffset && nodeTop < topOffset + windowHeight) || (nodeTop + nodeHeight > topOffset && nodeTop + nodeHeight < topOffset + windowHeight)) {
				if ((nodeLeft > leftOffset && nodeLeft < leftOffset + windowWidth) || (nodeLeft + nodeWidth > leftOffset && nodeLeft + nodeWidth < leftOffset + windowWidth))
					return true;
			}
				return false;
		}

# 当窗口滚动时，判断一个元素是不是出现在窗口可视范围。每次出现都在控制台打印 true

		$(window).on('scroll', function() {
			if (isVisible($node)) console.log(true);
		})
		
# 当窗口滚动时，判断一个元素是不是出现在窗口可视范围。在元素第一次出现时在控制台打印 true，以后再次出现不做任何处理

		$(window).on('scroll', function() {
			if (isVisible($node) && !$node.data('hasLog')) {
				console.log(true);
				$node.data('hasLog', true);
			}
		})
		
# 图片懒加载的原理是什么？

仅仅加载用户可以看到的区域，未被加载的图片符合某些条件或者触发了某些条件才开始异步加载




本博客版权归lidee92805和饥人谷所有，转载请注明出处





