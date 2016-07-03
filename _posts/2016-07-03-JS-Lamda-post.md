---
layout: post
title: "JavaScript闭包"
date: 2016-07-03
tags: [Web]
comments: true
---

# 什么是闭包？有什么作用？

闭包是指有权限访问另一个函数作用域的变量的函数，创建闭包的常见方式就是在一个函数内部创建另一个函数

* 闭包的作用

	* 可以读取函数内部的变量

	* 让这些变量的值始终保存在内存中

# setTimeout 0有什么作用

尽可能早的执行指定的任务

# 修改代码让fnArr\[i]() 输出 i

		var fnArr = [];
		for (var i = 0; i < 10; i++) {
			fnArr[i] = function() {
				return i;
			};
		}
		console.log(fnArr[3]());  // 10
		
* 方法一 闭包


		var fnArr = [];
		for (var i = 0; i < 10; i++) {
			fnArr[i] = (function(num) {
				return function() {
					return num;
				}
			}(i));
		};
		console.log(fnArr[3]()); // 3
		
* 方法二 对象绑定

		var fnArr = [];
		for (var i = 0; i < 10; i++) {
			var f = function() {
				
			};
			f.index = i;
			fnArr[i] = f;
		}
		console.log(fnArr[3].index);

# 使用闭包封装一个汽车对象，可以通过如下方式获取汽车状态

		var Car = (function() {
			var speed = 0;
			var status = 'stop';
			function setSpeed(s) {
				speed = s;
				if (speed > 0) {
					status = 'running';
				}
			};
			function getSpeed() {
				return speed;
			};
			function accelerate() {
				if (speed == 0) {
					status = 'running';
				}
				speed += 10;
			};
			function decelerate() {
				speed -= 10;
				if (speed <= 0) {
					status = 'stop';
					speed = 0;
				}
			}
			function getStatus() {
				return status;
			}
			return {setSpeed:setSpeed,
						getSpeed:getSpeed,
						accelerate:accelerate,
						decelerate:decelerate,
						getStatus:getStatus};
		}());

# 写一个函数使用setTimeout模拟setInterval的功能
	
		function mySetInterval(fn, delay) {
			setTimeout(function() {
				if (typeof fn == 'function')					fn();
				else if (typeof fn == 'string')					eval(fn);
				setTimeout(arguments.callee, delay);
			}, delay);
		}
		
# 写一个函数，计算setTimeout最小时间粒度

		function getMin(){
			var i = 0;
			var start = Date.now(); // 获取函数开始时间
			var clock = setTimeout(function(){ 
				i++; //1
				if(i === 1000){ //循环了一千次
					clearTimeout(clock); //延时函数停止
					var end = Date.now(); //获取函数结束时间
					console.log((end-start)/i); //获取每一次函数执行时间
				} else{ //i<1000时，调用setTimeout函数本身。
					clock = setTimeout(arguments.callee,0);
				}
			},0)
		}
		getMin();
		
# 分析代码

		var a = 1;
		setTimeout(function() {  //函数在本次所有代码执行完才会执行
			a = 2;
			console.log(a);
		}, 0);
		var a;
		console.log(a);
		a = 3;
		console.log(a);
		
		输出
		1
		3
		2
		
# 分析代码

		var flag = true;
		setTimeout(function() {  while是死循环执行不完，所以这里的方法不会执行
			flag = false;
		}, 0);
		while(flag){};
		console.log(flag);  
		
# 分析代码 如何输出delayer: 0, delayer:1...（使用闭包来实现）

		for (var i = 0; i < 5; i++) {
			setTimeout(function() {
				console.log('delayer:' + i);
			}, 0);
			console.log(i);
		}
		
		0 
		1 
		2 
		3 
		4 
		delayer:5
		delayer:5
		delayer:5
		delayer:5
		delayer:5
		
		for (var i = 0; i < 5; i++) {
			setTimeout((function(num) {
				return function() {
					console.log('delayer:' + num);
				};
			} (i)),0);
			console.log(i);
		}

本博客版权归lidee92805和饥人谷所有，转载请注明出处





