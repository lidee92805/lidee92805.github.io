---
layout: post
title: "对象、原型"
date: 2016-07-23
tags: [Web]
comments: true
---

# OOP 指什么？有哪些特性

* Object Oriented Programming,面向对象的程序设计

* 继承 使用现有类的所有功能，并在无需重新编写原来的类的情况下对这些功能进行扩展

* 封装 把客观事物封装成抽象的类

* 多态 允许将子类类型的指针赋值给父类类型的指针

# 如何通过构造函数的方式创建一个拥有属性和方法的对象

		function Dog(name) {
			this.name = name;
			this.bark = function() {
				console.log(this.name + '汪汪');
			}
		}
		
# prototype 是什么？有什么特性

* 每个函数都自动添加一个名称为prototype属性，这是一个对象

* prototype只是面对实例的，而不是面对类对象的

* 每个对象都有一个内部属性 __proto__(规范中没有指定这个名称，但是浏览器都这么实现的) 指向其类型的prototype属性，类的实例也是对象，其__proto__属性指向“类”的prototype

# 代码原型图

		function People(name) {
			this.name = name;
			this.sayName = function() {
				console.log('my name is:' + this.name);
			}
		}
		
		People.prototype.walk = function () {
			console.log(this.name + ' is walking');
		}
		
		var p1 = new People('饥人谷');
		var p2 = new People('前端');
		
![](/images/UML.png)

# 以下代码中的变量name有什么区别

		function People() {
			var name = "饥人谷";    //临时变量
			this.name = "我";      //实例变量
		}
		People.name = "jscode";   //方法名
		People.prototype.name = "学前端";   //原型对象变量
		可以通过实例变量访问原型对象变量，当实例变量和原型对象变量名相同，实例变量访问实例变量，原型对象变量访问原型对象变量
		
		


本博客版权归lidee92805和饥人谷所有，转载请注明出处





