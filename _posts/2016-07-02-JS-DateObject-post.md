---
layout: post
title: "JavaScript时间对象"
date: 2016-07-02
tags: [Web]
comments: true
---

# 基础类型有哪些？复杂类型有哪些？有什么特征？

* 基础类型

	1. Null

		Null类型只有一个值：null，表示空指针，也就是不存在的东西
	
	2. Undefined

		Undefined类型也只有一个值undefined，表示变量只被声明，没有被初始化，也就是有这个指针，但是这个指针没有指向任何空间
	
	3. Boolean

		Boolean有两个值，true和false
	
	4. Number

		JavaScript的数字类型和其它语言有所不同，没有整型和浮点数的区别，统一都是Number类型，可以表示十进制、八进制、十六进制
	
	5. String
	
		String是Unicode字符组成的序列，俗称字符串，可以用双引号或者单引号表示，没有区别，匹配即可
	
* 复杂类型

	1. Object

		一种无序的数据集合，由若干个“键值对”（key-value）构成。key我们称为对象的属性，value可以是任何JavaScript类型，甚至可以是对象
		
# 分析代码

		var obj1 = {a:1, b:2};
		var obj2 = {a:1, b:2};
		console.log(obj1 == obj2); //false
		console.log(obj1 = obj2); //Object {a: 1, b: 2}
		console.log(obj1 == obj2); //true

# 写一个函数getIntv，获取从当前时间到指定日期的间隔时间

		function getIntv(str) {
			var endDate = new Date(str);
			if (isNaN(endDate.getTime())) return;
			var nowDate = new Date();
			var daysLeft = endDate.getTime() - nowDate.getTime();
			return daysLeft = daysLeft / (24 * 60 * 60 * 1000);
		}
		var str = getIntv("2016-01-08");
		console.log(str);
		
# 把数字日期改成中文日期

		function getChsDate(str) {
			var fns = [function(date) {
				return date.getFullYear();
			},  function(date) {
				return date.getMonth() + 1;
			}, function(date) {
				return date.getDate();
			}];
			
			var charactors = [];
			var date = new Date(str);
			if (isNaN(date.getTime())) return;
			for (var i in fns) {
				var value =	fns[i](date);
				if (value >= 1970) {
					var yearStr = value.toString();
					for (var j in yearStr) {
						charactors.push(getChsCharactor(yearStr[j]));
					}
					charactors.push("年");
				} else {
					var tenPlace = value / 10;
					if (tenPlace >= 2) {
						charactors.push(getChsCharactor(tenPlace));
						charactors.push("十");
					}  else if (tenPlace == 1) {
						charactors.push("十");
					}
					charactors.push(getChsCharactor(value%10));
					if (i == 1) {
						charactors.push("月");
					} else if (i == 2) {
						charactors.push("日");
					}
				}
			}
			return charactors.join('');
		}
		function getChsCharactor(c) {
			var chs = ['零','一','二','三','四','五','六','七','八','九'];
			return chs[parseInt(c)];
		}
		
		var str = getChsDate('2015-01-08');
		console.log(str);
		
# 写一个函数获取n天前的日期

		function getLastNDays(day) {
			var now = new Date();
			return new Date(now.getTime() - day * 24 * 60 * 60 * 1000);
		}
		var lastWeek = getLastNDays(7);
		var lastMonth = getLastNDays(30);
		
# 获取执行时间

		var Runtime = (function() {
			
			return {
				start: function () {
					this.startDate = new Date();
				},
				end: function() {
					this.endDate = new Date();
				},
				get: function() {
					return this.endDate.getTime() - this.startDate.getTime();
				}
			};
		}());
		Runtime.start();
		for (var i = 0; i < 1000000000; i ++) {
			var j = 1;
		}
		Runtime.end();
		console.log(Runtime.get());
		
# 楼梯有200级，每次走1级或是2级，从底走到顶一共有多少种走法？用代码（递归）实现

		function upStairs(n) {
			if (n == 0 || n == 1) return 1;
			else return upStairs(n - 1) + upStairs(n - 2);
		}
		console.log(upStairs(10));
		
# 写一个json对象深拷贝的方法，json对象可以多层嵌套，值可以是字符串、数字、布尔、json对象中的任意项

		function copyObject(obj) {
			if (typeof obj[key] == 'object') {
				var cloneObj = {};
				for (var key in obj) {
					if (typeof obj[key] == 'object') {
					 	cloneObj[key] = copyObject(obj[key]);
					}
					else {
						cloneObj[key] = obj[key];
					}
				}
				return cloneObj;
			}
		}

# 写一个数组深拷贝的方法，数组里的值可以是字符串、数字、布尔、数组中的任意项目

		function copyArray(arr) {
			if (Object.prototype.toString.call(arr) === '[object Array]') {
				var cloneArr = [];
				for (var i = 0; i < arr.length; i++) {
					if (Object.prototype.toString.call(arr[i]) === '[object Array]') {
						cloneArr[i] = copyArray(arr[i]);
					} else {
						cloneArr[i] = arr[i];
					}
				}
				return cloneArr;
			}
		}


# 写一个深拷贝的方法，拷贝对象以及内部嵌套的值可以是字符串、数字、布尔、数组、json对象中的任意项

		function deepCopy(obj) {
			var clone = obj;
			if (typeof obj == 'object' && Object.prototype.toString.call(obj) === '[object Array]') {
				clone = [];
				for (var i = 0; i < obj.length; i++) {
					if (typeof obj[i] == 'object') {
					 	clone[i] = deepCopy(obj[i]);
					} else {
						clone[i] = obj[i];
					}
				}
			} else if (typeof obj == 'object') {
				clone = {};
				for (var key in obj) {
					if (typeof obj[key] == 'object') {
					 	clone[key] = deepCopy(obj[key]);
					}
					else {
						clone[key] = obj[key];
					}
				}
			}	
			return clone;
		}

本博客版权归lidee92805和饥人谷所有，转载请注明出处





