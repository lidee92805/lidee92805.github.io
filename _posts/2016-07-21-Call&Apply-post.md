---
layout: post
title: "this和闭包"
date: 2016-07-21
tags: [Web]
comments: true
---

# apply、call有什么作用，什么区别

* call() 方法在使用一个指定的this值和若干个指定的参数值的前提下调用某个函数或方法

* apply()方法在指定this值和参数（参数以数组或类似数组对象的形式存在）的情况下调用某个函数

* 两个方法只有一个区别，就是call()方法接受的是若干个参数的列表，而apply()方法接受的是一个包含多个参数的数组

# 分析代码1

		var john = {
			firstName: "John"
		}
		function func() {
			alert(this.firstName + ": hi!");
		}
		john.sayHi = func;
		john.sayHi();  //John hi!
		
# 分析代码2

		func()
		function func() {
			alert(this)   //window   window调用的func，所以this是window
		}
		
# 分析代码3

		function fn0() {
			function fn() {
				console.log(this); //window
			}
			fn();
		}		
		fn0();
		
		document.addEventListener('click', function(e) {
			console.log(this);  //document
			setTimeout(function() {
				console.log(this); //window
			}, 200);
		},false);

# 分析代码4

		var john = {
			firstName: "John"
		}
		function func() {
			alert(this.firstName); 
		}
		func.call(john);  // John  call传入this参数
		
# 分析代码5

		var john = {
			firstName: "John",
			surname: "Smith"
		}
		function func(a, b) {
			alert(this[a] + ' ' + this[b]);
		}
		func.call(john, 'firstName', 'surname');   // John Smith call分别传入this和a，b参数
		
# 分析代码6

		var module = {
			bind: function() {
				var me = this;
				$btn.on('click', function() {  
					//修改前
					console.log(this);//this指$btn
					this.showMsg();
					//修改后
					console.log(me);
					me.showMsg();
					
				})
			},
			showMsg: function() {
				console.log('饥人谷');
			}
		}

本博客版权归lidee92805和饥人谷所有，转载请注明出处





