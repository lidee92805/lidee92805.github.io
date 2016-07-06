---
layout: post
title: "正则表达式"
date: 2016-07-04
tags: [Web]
comments: true
---

# \d,\w,\s,[a-zA-Z0-9],\b,.,*,+,?,x{3},^$分别是什么？

* \d: 匹配数字

* \w: 匹配一个数字或字母字符或下划线或汉字

* \s: 匹配任何空白字符，包括空格、制表符、换页符等等

* [a-zA-z0-9]: 匹配一个数字或字母字符

* \b: 匹配一个字边界，及字与空格间的位置

* .: 匹配初换行符以外的任意字符

* *: 重复零次或更多次

* +: 重复一次或更多次

* ?: 重复零次或一次

* x{3}: x重复3次

* ^$: 匹配整个字符串

# 贪婪模式和非贪婪模式指什么？

* 贪婪模式：最大长度字符匹配

* 非贪婪模式：最小长度字符匹配

# 写一个函数trim(str),去除字符串两边的空白字符

		function trim(str) {
			return str.match(/\S.*\S/g);
		}

# 使用实现 addClass(el, cls)、hasClass(el, cls)、removeClass(el,cls)，使用正则

		function hasClass(el, cls) {
			var class = el.className;
			var exp = new RegExp('\\b'+cls+'\\b', 'g');
			return exp.test(class);
		}

		function addClass(el, cls) {
			var class = el.className;
			if(!hasClass(el, cls)) {
				class += " cls";
			}
		}

		function removeClass(el, cls) {
			var class = el.className;
			if (hasClass(el, cls)) {
				var exp = new RegExp(' '+cls, 'g');
				cls.replace(exp, "");
			}
		}
		
# 写一个函数isEmail(str)，判断用户输入的是不是邮箱

		function isEmail(str) {
			return /^\w+@{1}\w+[.]{1}\w+$/.test(str);
		}
		
#写一个函数isPhoneNum(str)，判断用户输入的是不是手机号

		function isPhoneNum(str) {
			return /^[1]{1}[35678]{1}\d{9}$/.test(str);
		}

# 写一个函数isValidUsername(str)，判断用户输入的是不是合法的用户名（长度6-20个字符，只能包括字母、数字、下划线）

		function isValidUsername(str) {
			return /^\w{6,20}$/.test(str);
		}


# 写一个函数isValidPassword(str)，判断用户输入的是不是合法密码（长度6-20个字符，包括大写字母、小写字母、数字、下划线至少两种）

		function isValidPassword(str) {
			if (/^\w{6,20}$/.test(str)) {
				if (/[a-z]+/.test(str)) {
					if (/[A-Z]+/.test(str) || /[0-9]+/.test(str) || /[_]+/.test(str)) 
						return true;
				} else if (/[A-Z]+/.test(str)) {
					if (/[0-9]+/.test(str) || /[_]+/.test(str)) 
						return true;
				} else if (/[0-9]+/.test(str)) {
					if(/[_]+/.test(str)) 
						return true;
				}
			}
			return false;
		}
		
# 写一个正则表达式，得到如下字符串里所有的颜色（#121212）

		var re = /[#]{1}\w{6}/g;
		
		var subj = 'color: #121212; background-color: #AA00ef'; width:12px; bad-colors: f#fddee #fd2';
		
		alert(subj.match(re)) //#121212,#AA00ef
		
# 改写代码，让其输出hunger, world

		var str = 'hello "hunger", hello "world"';
		var pat = /".*"/g;
		str.match(pat); //"hunger", hello "world" 贪婪模式最大匹配
		
		var pat = /".*?"/g;
		str.match(pat); "hunger" "world"
		
# 补全如下正则表达式，输出字符串中的注释内容. (可尝试使用贪婪模式和非贪婪模式两种方法)

		str = '.. <!-- My -- comment \n test --> ..  <!----> .. '
		re = /<!--[^>]*-->/  贪婪模式
		re = /<!--.*?-->>/   非贪婪模式
		
		str.match(re) // '<!-- My -- comment \n test -->', '<!---->'
		
# 补全如下正则表达式

		var re = /<[^>]+>/;
		
		var str = var str = '<> <a href="/"> <input type="radio" checked> <b>';
		str.match(re) // '<a href="/">', '<input type="radio" checked>', '<b>'
本博客版权归lidee92805和饥人谷所有，转载请注明出处





