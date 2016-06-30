---
layout: post
title: "JavaScript函数"
date: 2016-06-30
tags: [Web]
comments: true
---

# 函数声明和函数表达式有什么区别？

函数声明创建的函数可以在函数解析后调用（解析时进行逻辑处理）

		say("hello");  // 输出hello
		function say(a) {
			console.log(a);
		}
		say("world");  //输出world

函数表达式创建的函数是在运行时进行赋值，且要等到表达式赋值完成后才能调用

		say("hello"); //Error
		var say = function(a) {
			console.log(a);
		}
		say("world"); //输出world

# 什么是变量的声明前置？什么是函数的声明前置？

* 变量的声明前置

	当前作用域内的变量声明都会提升到作用域的最前面
	
			console.log(a);
			var a = 1;
			
			类似如下
			
			var a;
			console.log(a);
			a = 1;
			
* 函数的声明前置

	当前作用域的函数声明都会提升到作用域的最前面，虽然函数表达式也被前置，但前置后函数表达式的值为undefined，真正的初始值是在函数表达式被赋值之后
	
			f1();
			f2();
			var f1 = function(){};
			function f2() {}
			
			类似如下
			
			var f1;
			function f2() {}
			f1();
			f2()
			f1 = function(){};
			
# arguments是什么

在函数内部,可以使用arguments对象获取到该函数的所有传入参数

# 函数的重载怎样实现？

通过函数代码判断参数值和类型实现重载

	function printPeopleInfo(name, age, sex) {
		if (name) {
			console.log(name);
		}
		if (age) {
			console.log(age);
		}
		if (sex) {
			console.log(sex);
		}
	}
	printPeopleInfo('Byron', 26);
	printPeopleInfo('Byron', 26, 'male');

# 立即执行函数表达式是什么？有什么作用？

* 立即执行函数表达式是命名的或者匿名的函数表达式，在创建后立即执行

		1.(function() {
			console.log('hello world');
		}());  推荐写法
		2.(function() {
			console.log('hello world');
		})();
		
* 立即执行函数的作用

	* 不必为函数命名，避免了污染全局变量；

	* 立即执行函数执行完后就会销毁，不会对作用域存在影响；

	* 在立即执行函数内部形成一个单独的作用域，可以封装一些外部无法读取的私有变量；

# 什么是函数的作用域链？

* 函数的作用域链是由一系列对象(函数的活动对象+0个到多个的上层函数的活动对象+最后的全局对象)组成的,在函数执行的时候,会按照先后顺序从这些对象的属性中寻找函数体中用到的标识符的值(标识符解析).函数会在定义时将它们各自所处环境(全局上下文或者函数上下文)的作用域链存储到自身的[[scope]]内部属性中.

	

本博客版权归lidee92805和饥人谷所有，转载请注明出处





