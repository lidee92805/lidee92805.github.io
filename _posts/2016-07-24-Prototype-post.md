---
layout: post
title: "原型链"
date: 2016-07-24
tags: [Web]
comments: true
---

# 根据如下代码，解释Person、prototype、__proto__、p、constructor之间的关联

		function Person(name) {
			this.name = name;
		}		
		Person.prototype.sayName = function() {
			console.log('My name is :' + this.name);
		}
		var p = new Person('若愚');
		p.sayName();
		
![](/images/Property-Relation.png)

# 上例中，对对象 p可以调用 p.toString()，为什么？

![](/images/Prototype-link.png)

在 javaScript 中，每个对象都有一个指向它的原型（prototype）对象的内部链接。这个原型对象又有自己的原型，直到某个对象的原型为 null 为止（也就是不再有原型指向），组成这条链的最后一环。这种一级一级的链结构就称为原型链（prototype chain）

# 对String做扩展，实现方法获取字符串中频率最高的字符
		
		String.prototype.getMostOften = function() {
			var dic = {};
			for (var i = 0; i < this.length; i++) {
				var ch = this.substr(i, 1);
				dic[ch] = dic[ch] ? dic[ch] + 1 : 1;
			}
			var max,ch;
			for (var key in dic) {
				max = max || dic[key];
				ch = ch || key;
				if (dic[key] > max)
					ch = key;
			}
			return ch;
		}

# instanceOf有什么作用？内部逻辑是如何实现的？

* 判断一个对象是不是某个类型的实例

* 查看对象B的prototype指向的对象是否在对象A的[[prototype]]链上。如果在，则返回true,如果不在则返回false

本博客版权归lidee92805和饥人谷所有，转载请注明出处





