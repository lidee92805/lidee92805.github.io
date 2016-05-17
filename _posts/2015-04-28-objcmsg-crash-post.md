---
layout: post
title: "objc_msgSend在64位中崩溃的问题"
date: 2015-04-28
tags: [iOS]
comments: true
---

在适配64位的时候,程序会在objcmsgSend处崩溃,根据苹果的官方文档需要转换

	－（int）doSomething:(int)x {...}
	- (void)doSomethingElse {
		int (action)(id, SEL, int) = (int()(id, SEL, int)) objcmsgSend;
		action(self, @selector(doSomething:), 0);
	}

[苹果官方文档地址](https://developer.apple.com/library/ios/documentation/General/Conceptual/CocoaTouch64BitGuide/ConvertingYourAppto64-Bit/ConvertingYourAppto64-Bit.html)



