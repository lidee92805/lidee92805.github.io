---
layout: post
title: "RequireJS"
date: 2016-07-26
tags: [Web]
comments: true
---

# 如下requirejs配置中, baseUrl 有什么作用？以什么作为基准? paths 的作用和用法是什么?

		requirejs.config({
			baseUrl: 'src/js',  //相对地址加载所有代码  基准为包含RequireJS的那个HTML页面的所有目录
			paths: { //  设置相对于baseUrl地址的模块ID
				'jquery': 'lib/bower_components/jquery/dist/jquery.min'
			}
		})

# 如下 r.js 的打包配置中 baseUrl 是什么? name 是什么?

		({
			baseUrl: "./src/js",  查找的基础目录
			paths: {
				'jquery': 'lib/bower_components/jquery/dist/jqury.min'
			},
			name: "main",  解析入口相对于baseUrl
			out: "dist/js/merge.js"
		})


本博客版权归lidee92805和饥人谷所有，转载请注明出处





