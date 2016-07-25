---
layout: post
title: "继承"
date: 2016-07-24
tags: [Web]
comments: true
---

# 继承有什么作用?

* 使用现有类的所有功能，并在无需重新编写原来的类的情况下对这些功能进行扩展

# 有几种常见创建对象的方式? 举例说明?

1. 原始方法

		var obj = new Object();
		obj.name = 'li';
		obj.hi = function() {
			console.log(this.name);
		}
		
2. 工厂方法

		function createObj() {
			var obj = new Object();
			obj.name = 'li';
			obj.hi = function() {
				console.log(this.name);
			}
			return obj;
		}
		var obj = createObj();
		
3. 构造函数

		function obj(name) {
			this.name = name;
			this.hi = function() {
				console.log(this.name);
			}
		}
		var obj = new obj('li');
		
4. Prototype

		function obj(name) {
			obj.prototype.name = name;
			obj.prototype.hi = function() {
				console.log(this.name);
			}
		}
		var obj = new obj('li');

# 下面两种写法有什么区别?

		//方法1
		function People(name, sex) {
			this.name = name;
			this.sex = sex;
			this.printName = function() {
				console.log(this.name);
			}
		}
		var p1 = new People('饥人谷', 2);
		
		//方法2
		function Person(name, sex) {
			this.name = name;
			this.sex = sex;
		}
		Person.prototype.printName = function() {
			console.log(this.name);
		}
		var p1 = new Person('若愚', 27);
		方法1只能被实例访问，可以用来不想被子类访问的属性和方法
		方法2通过原型访问，可以被实例和子类访问

# Object.create 有什么作用？兼容性如何？如何使用？

* Object.create() 方法创建一个拥有指定原型和若干个指定属性的对象

![](/images/ObjectCreate.png)

* Object.create(proto, [ propertiesObject ])

	* proto 
	  一个对象，作为新创建对象的原型。

	* propertiesObject
可选。该参数对象是一组属性与值，该对象的属性名称将是新创建的对象的属性名称，值是属性描述符（这些属性描述符的结构与Object.defineProperties()的第二个参数一样）。注意：该参数对象不能是 undefined，另外只有该对象中自身拥有的可枚举的属性才有效，也就是说该对象的原型链上属性是无效的

# hasOwnProperty有什么作用？ 如何使用？

* 用来判断某个对象是否含有指定的自身属性

* obj.hasOwnProperty(prop)

	* prop
要检测的属性名称

# 实现Object.create的 polyfill

		function create(obj) {
			var newObj;
			if (typeof Object.create === 'function')
				return Object.create(obj);
			else {
				function fn() {};
				fn.prototype = obj;
				newObj = new fn();
			}
			if (arguments.length > 1) {
				for (var key in arguments[1]) {
					newObj[key] = arguments[1][key]['value'];
				}
			}
			return newObj;
		}
		
# 如下代码中call的作用是什么?

		function Person(name, sex) {
			this.name = name; //call传入Male实例的运行环境，这里的this为Male实例
			this.sex = sex;
		}
		function Male(name, sex, age) {
			Person.call(this, name, sex);
			this.age = age;
		}
		
# 补全代码，实现继承

		function Person(name, sex) {
			this.name = name;
			this.sex = sex;
		}
		
		Person.prototype.printName = function() {
			console.log(this.name);
		};
		
		function Male(name, sex, age) {
			Person.call(this, name, sex);
			this.age = age;
			this.constructor = Male;
		}
		Male.prototype = Object.create(Person.prototype);
		Male.prototype.getAge = function() {
			return this.age;
		};
		var ruoyu = new Male('若愚', '男', 27);
		ruoyu.printName();


本博客版权归lidee92805和饥人谷所有，转载请注明出处





