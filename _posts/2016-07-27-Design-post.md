---
layout: post
title: "设计模式"
date: 2016-07-27
tags: [Web]
comments: true
---

# 写出 构造函数模式、混合模式、模块模式、工厂模式、单例模式、发布订阅模式的范例

* 构造函数

		function Person(name, age) {
			this.name = name;
			this.age = age;
		}
		var p = new Person('Li', 24);
		
* 混合模式

		function Animal(name) {
			this.name = name;
		}
		Animal.prototype.eat = function() {
			console.log('eat food');
		}
		function create(obj) {
			function fn(){};
			fn.prototype = obj;
			return new fn;
		}
		function Dog(name) {
			Animal.call(this, name);
		}
		Dog.prototype = create(Animal.prototype);
		Dog.prototype.bark = function() {
			console.log('wang wang');
		}
		
* 模块模式

		var Car = (function(){
			var speed = 0;
			function speedUp() {
				speed+=10;
			}
			function speedDown() {
				speed-=10;
				if (speed < 0) speed = 0;
			}
			function getSpeed() {
				return speed;
			}
			return {
				speedUp: speedUp,
				speedDown: speedDown,
				getSpeed: getSpeed
			}
		})();
		
		Car.speedUp();
		Car.speedDown();
		Car.getSpeed();
		
* 工厂模式

		function createPerson(name, age) {
			return {
				name: name,
				age: age
			}
		}
		var p = createPerson('Li', 24);
		
* 单例模式

		var Manager = (function(){
			var instance;
			function Manager() {
				this.maxCount = 10;
			}
			if (!instance) instance = new Manager();
			return instance;
		})();

# 使用发布订阅模式写一个事件管理器

		var EventManager = (function() {
        var store = {};
        function on(evt, handler) {
          store[evt] = store[evt] || [];
          var fun = {};
          fun['name'] = handler;
          fun['args'] = [].slice.call(arguments, 2);
          store[evt].push(fun);
        }
        function fire(evt) {
          var array = store[evt];
          for (var i = 0; i < array.length; i++) {
            var fun = array[i];
            fun['name'].apply(this, fun['args']);
          }
        }
        function off(evt, handler) { 
          if (arguments.length < 2)
            delete store[evt];
          else {
            var handlerStr = funToString(handler.toString());
            for (var i = 0; i < store[evt].length; i++) {
              var fun = store[evt][i];
              if (handlerStr === funToString(fun['name'].toString())) {
                store[evt].splice(i, 1);
                return;
              }
            }
          }
        }
        function funToString(fun) {
          fun = fun.replace(/[ | ]*\n/g,'\n'); //去除行尾空白
          fun = fun.replace(/\n[\s| | ]*\r/g,'\n'); //去除多余空行
          fun = fun.replace(/ /ig,'');//去掉
          fun = fun.replace(/^[\s　]+|[\s　]+$/g, "");//去掉全角半角空格
          fun = fun.replace(/[\r\n]/g,"");//去掉回车换行
          return fun;
        }
        return {
          on: on,
          fire: fire,
          off: off
        }
      })();
      EventManager.on('test', function() {
        console.log('test1');
      });
      EventManager.on('test', function() {
        console.log('test2');
      });
      EventManager.on('test', function(str) {
        console.log(str + 'test2');
      }, 'My');
      EventManager.on('test', function(str, str2) {
        console.log(str + 'test' + str2);
      }, 'Li', 'hello');
      EventManager.fire('test');
      console.log('-----------------------------');
      EventManager.off('test', function(str) {
        console.log(str + 'test2');
      });
      EventManager.fire('test');
      console.log('-----------------------------');
      EventManager.off('test', function() {
        console.log('test2');
      });
      EventManager.fire('test');

# 


本博客版权归lidee92805和饥人谷所有，转载请注明出处





