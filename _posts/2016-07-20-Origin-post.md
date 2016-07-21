---
layout: post
title: "同源策略和跨源处理"
date: 2016-07-18
tags: [Web]
comments: true
---

# 什么是同源策略

* 同源策略限制了一个源（origin）中加载文本或脚本与来自其它源（origin）中资源的交互方式

# 什么是跨域?跨域有几种实现形式

* 跨域是突破同源策略的限制

* 跨域形式

	* 降域

		* 在根域范围内，允许把domain属性的值设置为它的上一级域

		* 这两个域名必须属于同一个基础域名!而且所用的协议，端口都要一致，否则无法利用document.domain进行跨域。
		
		* 两个页面都要加入document.domain = 'xxx.com'

	* JSONP

		* 原理是动态加载一个js文件，传入回调方法名，调用的js文件调用回调方法并传入参数

		* 只能进行GET请求

		* 可被注入代码

		* 需要校验身份

	* CORS

	 	* 支持跨站访问控制，从而使得安全地进行跨站数据传输成为可能

	 	* 服务器声明哪些来源可以通过浏览器访问该服务器上的资源

	* HTML5 postMessage

		* postMessage()方法允许来自不同源的脚本采用异步方式进行有限的通信，可以实现跨文本档、多窗口、跨域消息传递

		* postMessage(data,origin)方法接受两个参数
 
			 1. data:要传递的数据，html5规范中提到该参数可以是JavaScript的任意基本类型或可复制的对象，然而并不是所有浏览器都做到了这点儿，部分浏览器只能处理字符串参数，所以我们在传递参数的时候需要使用JSON.stringify()方法对对象参数序列化，在低版本IE中引用json2.js可以实现类似效果。
 
			2. origin：字符串参数，指明目标窗口的源，协议+主机+端口号[+URL]，URL会被忽略，所以可以不写，这个参数是为了安全考虑，postMessage()方法只会将message传递给指定窗口，当然如果愿意也可以建参数设置为"*"，这样可以传递给任意窗口，如果要指定和当前窗口同源的话设置为"/"。

	* hash
		
		* 利用location.hash来进行传值。在url： http://a.com#helloword 中的‘#helloworld’就是location.hash，改变hash并不会导致页面刷新，所以可以利用hash值来进行数据传递，当然数据容量是有限的。假设域名a.com下的文件cs1.html要和cnblogs.com域名下的cs2.html传递信息，cs1.html首先创建自动创建一个隐藏的iframe，iframe的src指向cnblogs.com域名下的cs2.html页面，这时的hash值可以做参数传递用。cs2.html响应请求后再将通过修改cs1.html的hash值来传递数据（由于两个页面不在同一个域下IE、Chrome不允许修改parent.location.hash的值，所以要借助于a.com域名下的一个代理iframe；Firefox可以修改）。同时在cs1.html上加一个定时器，隔一段时间来判断location.hash的值有没有变化，一点有变化则获取获取hash值
		
	* window.name

		* iframe的src属性由外域转向本地域，跨域数据即由iframe的window.name从外域传递到本地域。这个就巧妙地绕过了浏览器的跨域访问限制，但同时它又是安全操作
	
			1. 在应用页面（a.com/app.html）中创建一个iframe，把其src指向数据页面（b.com/data.html）。
数据页面会把数据附加到这个iframe的window.name上

			2. 在应用页面（a.com/app.html）中监听iframe的onload事件，在此事件中设置这个iframe的src指向本地域的代理文件（代理文件和应用页面在同一域下，所以可以相互通信）

			3. 获取数据以后销毁这个iframe，释放内存；这也保证了安全（不被其他域frame js访问）。

		

本博客版权归lidee92805和饥人谷所有，转载请注明出处





