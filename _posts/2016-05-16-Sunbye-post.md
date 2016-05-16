---
layout: post
title: 浏览器基础笔记
category: 前端开发
date: 2016-05-16
---
# 网页乱码的问题是如何产生的？怎样解决
* 网页乱码是由于html文件的编码和解码不匹配造成的
* 解决办法：在head标签里添加<meta charset="编码方式">，注明文件的编码方式
# 颜色有几种写法
* 颜色单词，十六进制颜色值，RGB方式
# <!doctype html>的作用是什么
* 声明按照html5的标准进行页面渲染
# 严格模式和混杂模式指什么
* 严格模式：浏览器以其支持的最高标准呈现页面
* 混杂模式：页面以一种比较宽松的向后兼容的方式显示混杂模式通常模拟老式浏览器的行为以防止老站点无法工作
# meta有什么作用，常见的值有哪些
<pre><code>Meta elements are typically used to specify page description,keywords,author of the document,last modified,and other metadata.
The metadata can be used by browsers(how to display content or reload page),search engines(keywords),or other web services</code></pre>
<pre><code>meta常用于定义页面的说明，关键字，最后修改日期和其他元数据。
这些元数据将服务于浏览器（如何布局或重载页面），搜索引擎和其他网络服务。</code></pre>
* name属性
* name属性主要用于描述网页，比如网页的关键词，叙述等。与之对应的属性值为content，content中的内容是对name填入类型的具体描述，便于搜索引擎抓取。
meta标签中name属性语法格式是：
    <pre><code>meta name="参数" content="具体的描述"</code></pre>
    * A.keywords(关键字)
    * 说明：用于告诉搜索引擎，网页的关键字。
    <pre><code>meta name="keywords" content="前端知识"</code></pre>
    * B.description(网站内容的描述)
    * 说明：用于告诉搜索引擎，网站的主要内容
    <pre><code>meta name="descritption" content="这篇文章主要介绍一些前端的知识"</code></pre>
    * C.robots(定义搜索引擎爬虫的索引方式)
    * robots用来告诉爬虫哪些页面需要索引，哪些页面不需要索引。
content的参数有all,none,index,noindex,follow,nofollow。默认是all
1.none : 搜索引擎将忽略此网页，等价于noindex，nofollow。
2.noindex : 搜索引擎不索引此网页。
3.nofollow: 搜索引擎不继续通过此网页的链接索引搜索其它的网页。
4.all : 搜索引擎将索引此网页与继续通过此网页的链接索引，等价于index，follow。
5.index : 搜索引擎索引此网页。
6.follow : 搜索引擎继续通过此网页的链接索引搜索其它的网页。
    <pre><code>meta name="robots" content="none"</code></pre>
    * D.author(作者)
    * 用于标注网页作者
    <pre><code>meta name="author" content="732264588@qq.com"</code></pre>
    * E.copyright(版权)
    * 用于标注版权信息
    <pre><code>meta name="copyright" content="xxxx"</code></pre>
* http-equiv属性
* http-equiv顾名思义，相当于http的文件头作用，它可以向浏览器传回一些有用的信息，以帮助正确和精确地显示网页内容，与之对应的属性值为content，content中的内容其实就是各个参数的变量值。
    <pre><code>meta http-equiv="参数" content="具体的描述"</code></pre>
    * A. content-Type(设定网页字符集)(推荐使用HTML5的方式)
    * 说明：用于设定网页字符集，便于浏览器解析与渲染页面
    <pre><code>meta charset="utf-8"</code></pre>
    * X-UA-Compatible(浏览器采取何种版本渲染当前页面)
    * 说明：用于告知浏览器以何种版本来渲染页面。（一般都设置为最新模式，在各大框架中这个设置也很常见。）
    <pre><code>meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"</code></pre>
    * cache-control(指定请求和响应遵循的缓存机制)
    * 指导浏览器如何缓存某个响应以及缓存多长时间。
no-cache: 先发送请求，与服务器确认该资源是否被更改，如果未被更改，则使用缓存。
no-store: 不允许缓存，每次都要去服务器上，下载完整的响应。（安全措施）
public : 缓存所有响应，但并非必须。因为max-age也可以做到相同效果
private : 只为单个用户缓存，因此不允许任何中继进行缓存。（比如说CDN就不允许缓存private的响应）
maxage : 表示当前请求开始，该响应在多久内能被缓存和重用，而不去服务器重新请求。例如：max-age=60表示响应可以再缓存和重用 60 秒。
    <pre><code>meta http-equiv="cache-control" content="no-cache"</code></pre>
    * D. expires(网页到期时间)
    * 说明：用于设定网页的到期时间，过期后网页必须到服务器上重新传输。
    <pre><code>meta http-equiv="expires" content="Monday 16 May 2016 21:00 GMT"</code></pre>
    * E. refresh(自动刷新并指向某页面)
    * 说明：网页将在设定的时间内，自动刷新并调向设定的网址。
    <pre><code>meta http-equiv="refresh" content="2; URL=http://www.baidu.com" 2秒后跳转百度</code></pre>
    * Set-Cookie(cookie设定)
    * 说明：如果网页过期。那么这个网页存在本地的cookies也会被自动删除。
    <pre><code>meta http-equiv="Set-Cookie" content="name, date"
meta http-equiv="Set-Cookie" content="User=xxxx; path=/; expires=Sunday, 10-Jan-16 10:00:00 GMT"</code></pre>
# 常见的浏览器有哪些，什么内核
* Internet Explorer浏览器：内核为Trident
Chrome 浏览器： 内核为WebKit
Firefox火狐浏览器：内核为Gecko
Safari浏览器：内核为WebKit
Opera浏览器： 内核为Presto

