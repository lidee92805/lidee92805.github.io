---
layout: post
title: "jQuery源码浅析"
date: 2016-08-02
tags: [Web]
comments: true
---

jQuery是目前最受欢迎的JavaScript库，有文件小、方便的选择器、链式操作、简化AJAX、丰富的插件等诸多特点。现在让我们来探索一下jQuery的源码，看一看大致的实现原理，以下内容基于jQuery的3.1.0版本

* jQuery库的本质

	下载未压缩过的jQuery打开看看，最开始的地方是这个
	
			(function(global, factory) {
			
				//blabla
				
			})(typeof window !== 'undefined' ? window : this, function(window, noGlobal) {
			
				//blabla
				
			});
			
	搜索一下noGlobal参数，只在整个文件的最末尾用到了，所以jQuery库只是一个global为window或者this的global，factory是一个有一万行代码的方法的立即执行函数，在这个立即执行函数中对CommonJS和类CommonJS的环境做了处理
	
* $

	jQuery中最常使用的就是$符号，这是什么呢？
	
		if (!noGlobal) {
			window.jQuery = window.$ = jQuery; 
		}
		
	$和jQuery都是window的属性赋值为jQuery，那么赋值的jQuery又是什么呢？
	
		var jQuery = function(selector, context) {
			return new jQuery.fn.init(selector, context);
		}
		
	jQuery传入了2个参数然后新创建对象返回，这样jQuery就实现了外部不需要new创建对象，再来看一下jQuery.fn.init是什么
	
		jQuery.fn = jQuery.prototype = {
			//定义了一些方法
		}
		
	jQuery.fn指向了jQuery的原型，在原型中定义了一些类数组的方法。所以当我们写jQuery的插件的时候，是给jQuery对象的原型添加方法，jQuery对象也能够进行调用
	
		init = jQuery.fn.init = function(selector, context, root) {
			...
		}
		init.prototype = jQuery.fn; // jQuery.fn指向jQuery的原型，保证创建的对象能访问jQuery的原型
		
	init方法用于创建jQuery对象，并且解析选择器找到DOM中的元素。
	
	所以$的作用是使用init方法创建jQuery对象然后返回
	
* jQuery对象和DOM对象的关系

	先来看看init方法中的一段代码
	
			init = jQuery.fn.init = function(selector, context, root) {
				...
				elem = document.getElementById(match[2]);
				if (elem) {
					this[0] = elem;
					this.length = 1;
				}
				return this;
			}

	这段代码说明了jQuery对象和DOM对象的关系，把DOM对象像插入数组一样直接插入jQuery对象中，这也解释了为什么$('li')[0]这样取到的是DOM对象。然后再来看看eq()方法
	
			在jQuery对象的原型中
			eq: function(i) {
				var len = this.length,
					j = +i (i < 0? len : 0);
				return this.pushStack( j >= 0 && j < len ? [this[j]] : [] );
			}
			
			pushStack: function(elems) {
				var ret = jQuery.merge(this.constructor(), elems);
				ret.prevObject = this;
				return ret;
			}
			
			merge: function(first, second) {
				var len = +second.length,
					j = 0,
					i = first.length;
					
				for ( ; j < len; j++ ) {
					first[i++] = second[j];
				}
				first.length = i;
				return first;
			}
			
	eq方法中先找到对应的DOM对象，然后传给pushStack方法，然后调用this的constructor方法，这里的this是jQuery对象，jQuery的constructor方法是创建对象的方法，pushStack会创建一个空的jQuery对象，然后把jQuery对象和DOM对象传给merge方法，merge方法把DOM对象插入到jQuery对象中
	
* extend

	jQuery源码中大量使用了extend方法
	
		jQuery.extend = jQuery.fn.extend = function() {
			...
			if (deep && copy && (jQuery.isPlainObject(copy) || (copyIsArray = jQuery.isArray(copy)))) {
				if (copyIsArray) {
					copyIsArray = false;
					clone = src && jQuery.isArray(src) ? src : [];
				} else {
					clone = src && jQuery.isPlainObject(src) ? src : {};
				}
				target[name] = jQuery.extend(deep, clone, copy);
			} else if (copy !== undefined) {
				target[name] = copy;
			}
			...
		}
		
	只贴出了关键的代码，可以看到extend方法只是一个深拷贝的方法。
	



本博客版权归lidee92805和饥人谷所有，转载请注明出处





