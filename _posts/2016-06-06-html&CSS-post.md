---
layout: post
title: "html&CSS知识"
date: 2016-06-06
tags: [Web]
comments: true
---

# 有序列表、无序列表、自定义列表如何使用？三者在语意上有什么区别？在哪种情况下使用哪种（重要）？如何嵌套？
* 有序列表：表示一些相关项的列表并且每项的顺序很重要

		<ol>
			<li>打开浏览器</li>
			<li>输入网址</li>
			<li>浏览信息</li>
		</ol>
	
	<ol>
		<li>打开浏览器</li>
		<li>输入网址</li>
		<li>浏览信息</li>
	</ol>
	
* 无序列表：表示一些相关项的列表，每项的顺序不重要或者没有编号或者不以字母顺序为序

		<ul>
			<li>iPad</li>
			<li>iPhone</li>
			<li>Macbook pro</li>
		</ul>
		
	<ul>
		<li>iPad</li>
		<li>iPhone</li>
		<li>Macbook pro</li>
	</ul>	
	
* 自定义列表：概述多个极其描述，通常是专业集。一个自定义列表可能会有多个定义和描述。另外，每个描述可能对应多个术语，每个术语也可能有多个描述，术语必须在描述之前。

		<dl>
			<dt>术语一</dt>
			<dd>描述一</dd>
			<dt>术语二</dt>
			<dd>描述二</dd>
			<dd>描述二.二</dd>
			<dt>术语三</dt>
			<dt>术语三.二</dt>
			<dd>描述三</dd>
		</dl>
	
	<dl>
		<dt>术语一</dt>
		<dd>描述一</dd>
		<dt>术语二</dt>
		<dd>描述二</dd>
		<dd>描述二.二</dd>
		<dt>术语三</dt>
		<dt>术语三.二</dt>
		<dd>描述三</dd>
	</dl>
	
* 列表嵌套：列表间容许嵌套，在闭合列表标签前插入要嵌套的列表，之后再闭合列表标签继续原来的列表

		<ul>
			<li>看电影</li>
			<li>网上冲浪
				<ol>
					<li>打开浏览器</li>
					<li>输入网址</li>
					<li>浏览信息</li>
				</ol>
			</li>
			<li>玩游戏</li>
		</ul>
		
	<ul>
		<li>看电影</li>
		<li>网上冲浪
			<ol>
				<li>打开浏览器</li>
				<li>输入网址</li>
				<li>浏览信息</li>
			</ol>
		</li>
		<li>玩游戏</li>
	</ul>

# 如何去除列表前面的点或者数字
* 清楚列表默认样式：list-style: none;

# class和id有什么区别？什么时候用class什么时候用id？
* id选择器在一个html文件中只能被使用一次，而class选择器在一个html文件中可以被使用多次。id选择器可以被js中的GetElementByID函数所运用，class不行。
* 页面大的模组，用id来区分模组；用js的GetElementByID函数时用id，其他情况用class

# 块级元素、行内元素是什么？有什么区别？分别对应哪些常用的标签？
* 块级元素占空间是一整行，行内元素占用空间是它自身的内容高度。行内元素可以并排显示，块级元素不能。
* 块级元素常用标签：div、p、h1...h6、table、tr、ul、li、dl、dt、form
* 行内元素常用标签：a、span、img、input、button、em、textarea

# display: block、display: inline、diaplay: inline-block分别有什么作用？
* display: block 元素在父元素内尽可能的占据最多的宽度直到和父元素等宽
* display: inline 元素尽可能占据最小的空间，垂直方向的padding失效，水平方向的padding生效
* display: inline-block 元素在占据最小的空间情况下所有的padding全都生效

# 分析代码

	<!DOCTYPE html>
	<html>
	<head>
  	   <meta charset="utf-8">
  		<style>
    		.wrap{
      		width: 900px;
      		margin: 0 auto;
    		}
  
  		</style>
	</head>
	<body>
  		<div id="header">
    		<div class="wrap">
      			<a id="logo" href="#"><img src=""></a>
      			<ul class="nav">
        				<li><a href="#">导航1</a></li>
        				<li><a href="#">导航2</a></li>
        				<li><a href="#">导航3</a></li>
      			</ul>
    		</div>
 		</div>
  		<div id="content">
    		<div class="wrap">
      			<div class="aside">侧边栏</div>
      			<div class="main">中心区块</div>
    		</div>
  		</div>
		<div id="footer">
  			<div class="wrap">这里是 footer</div>
		</div>
	</body>
	</html>
	
为所有class为wrap的标签添加样式

# 如何理解HTML CSS语义化？在平时写代码的过程中要注意哪些细节
* HTML语义化：用合理HTML标记及其特有的属性去格式化文档内容，能不用div的地方不用div，充分利用html标签
* CSS语义化：用易于理解的名称对html标签附加的class或id命名。
	* 尽量规避拼音命名，用英文单词命名
	* 单词之间连接用三种方式：下划线_、间隔符-、驼峰命名。命名方式尽量统一
	* 单词后不要接无意义的数字

# form表单有什么作用？有哪些常用的input标签，分别有什么作用？
* form用于把用户输入的数据提交到后台
* type="text"：用于输入文本。placeholder属性（可选）展示的是输入框里的提示，maxlength（可选）限制最大输入长度
	
		<input name="username" type="text" placeholder="用户名" maxlenght=10/>
		
* type="password"：用于输入密码，输入的内容显示为星号

		<input name="password" type="password" placeholder="密码"/>
		
* type="radio"：单选圆圈按钮。注意：name要相同才能实现单选，value要有值

		<input type="radio" name="sex" value="male"/>男
		<input type="radio" name="sex" value="female"/>女
		
* type="checkbox"：复选框。加checked属性会默认选上。提交时，如果选中（如bike），则bike=on

		<input type="checkbox" name="bike" checked/>自行车
		<input type="checkbox" name="car"/>汽车
		
* type="textarea"：文本域，用于输入多行文本

		<textarea name="maneywords" maxlength=10 placeholder="ddd"></textarea>
		
* type="hidden"：隐藏域，用户看不到，用于暂存数据。或者安全校验

		<input name="url_delegate" type="hidden" value="/delegate.php"/>
		<input name="csrf_token" type="hidden" value="adufiqqwero132"/>

# post和get方式的区别
1. 数据的提交方式不同，get提交的数据url可以看到，post看不到
2. get一般用于提交少量数据，post用于提交大量数据
3. get最多提交1k数据，浏览器的限制。post理论上无限制，受服务器限制
4. get提交的数据在浏览器历史纪录中，安全性不好

# 在input中，name有什么作用
提供了一种在脚本中引用表单的方法

# ≺button≻提交≺/button≻、≺a class="btn" href="#"≻提交≺/a≻、≺input type="submit" value="提交"／≻三者有什么区别

* ≺button≻提交≺/button≻按钮的内容不光可以有文字还可以有图片等多媒体内容，但是不同的浏览器得到的value值不同，有兼容性问题
* ≺a class="btn" href="#"≻提交≺/a≻超链接可以伪装成按钮
* ≺input type="submit" value="提交"／≻点击自动提交表单内容

# radio如何分组
* name相同的为一组

# placeholder属性有什么作用
* 展示输入框里的提示

# type=hidden隐藏域有什么作用
* 用于暂存数据或者安全校验

		<input name="user-token" type="hidden" value="this is the token"/>
		
	得到user-token的值用于验证
	
	
	
本博客版权归lidee92805和饥人谷所有，转载请注明出处


