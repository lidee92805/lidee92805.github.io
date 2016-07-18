---
layout: post
title: "熟悉后端语言"
date: 2016-07-07
tags: [Web]
comments: true
---

# 简单描述下web服务器、php、数据库、浏览器是如何实现动态网站的？

1. 通过本机配置好的DNS域名服务器地址寻找DNS服务器，将网站URL中的Web主机域名解析为Web服务器所在的Linux操作系统（Apache通常与Linux操作系统组合使用）中对应的IP地址。

2. 通过HTTP协议（超文本传输协议）去连接上述IP地址的服务器系统，请求Apache服务器上相应目录下的php文件

3. Apache服务器本身不能处理php动态语言脚本文件，就寻找并委托PHP应用服务器来处理

4. PHP应用服务器打开php文件，在php文件中通过对数据库连接的代码来连接本机或者网络上其他机器上的MySQL数据库，并在php程序中通过执行标准的SQL查询语句来获取数据库中的数据，再通过PHP应用服务器将数据生成html静态代码

5. 浏览器收到Web服务器的响应后，接收并下载服务器端的html静态代码，然后浏览器解读代码，最终将网页呈现出来

# 常见的WEB服务器有哪些？

* Apache

	Apache是世界使用排名第一的Web服务器软件。它可以运行在几乎所有广泛使用的计算机平台上，由于其跨平台和安全性被广泛使用，是最流行的Web服务器端软件之一。它快速、可靠并且可通过简单的API扩充，将Perl/Python等解释器编译到服务器中。

* Nginx

	Nginx ("engine x") 是一个高性能的HTTP和 反向代理 服务器，也是一个 IMAP/POP3/SMTP服务器。 Nginx 是由 Igor Sysoev 为俄罗斯访问量第二的 Rambler.ru 站点开发的，第一个公开版本0.1.0发布于2004年10月4日。其将源代码以类BSD许可证的形式发布，因它的稳定性、丰富的功能集、示例配置文件和低系统资源的消耗而闻名。

* Lighttpd

	Lighttpd 是一个德国人领导的开源Web服务器软件，其根本的目的是提供一个专门针对高性能网站，安全、快速、兼容性好并且灵活的web server环境。具有非常低的内存开销、cpu占用率低、效能好以及丰富的模块等特点。 Lighttpd是众多OpenSource轻量级的web server中较为优秀的一个。支持FastCGI，CGI，Auth，输出压缩(output compress)，URL重写，Alias等重要功能

* Tomcat

	Tomcat 服务器是一个免费的开放源代码的Web 应用服务器，是Apache 软件基金会（Apache Software Foundation）的Jakarta 项目中的一个核心项目，由Apache、Sun 和其他一些公司及个人共同开发而成。由于有了Sun 的参与和支持，最新的Servlet 和JSP 规范总是能在Tomcat 中得到体现，Tomcat 5 支持最新的Servlet 2.4 和JSP 2.0 规范。因为Tomcat 技术先进、性能稳定，而且免费，因而深受Java 爱好者的喜爱并得到了部分软件开发商的认可，成为目前比较流行的Web 应用服务器。目前最新版本是8.0。

* IIS

	IIS是Internet Information Services的缩写，是一个World Wide Web server。Gopher server和FTP server全部包容在里面。 IIS意味着你能发布网页，并且有ASP（Active Server Pages）、JAVA、VBscript产生页面，有着一些扩展功能。IIS支持一些有趣的东西，像有编辑环境的界面（FRONTPAGE）、有全文检索功能的（INDEX SERVER）、有多媒体功能的（NET SHOW） 其次,IIS是随Windows NT Server 4.0一起提供的文件和应用程序服务器，是在Windows NT Server上建立Internet服务器的基本组件。它与Windows NT Server完全集成，允许使用Windows NT Server内置的安全性以及NTFS文件系统建立强大灵活的Internet/Intranet站点。IIS（Internet Information Server，互联网信息服务）是一种Web（网页）服务组件，其中包括Web服务器、FTP服务器、NNTP服务器和SMTP服务器，分别用于网页浏览、文件传输、新闻服务和邮件发送等方面，它使得在网络（包括互联网和局域网）上发布信息成了一件很容易的事。
	
# 打开浏览器，在地址栏输入 http://jirengu.com 页面展现了饥人谷官网的信息，整个过程发生了什么？（饥人谷官网后台语言 php,web服务器 nginx，数据库 mysql）

* 从浏览器到浏览器内核

	在这过程中，浏览器可能会做一些预处理，比如 Chrome 会根据历史统计来预估所输入字符对应的网站，比如输入了「ba」，根据之前的历史发现 90% 的概率会访问「www.baidu.com 」，因此就会在输入回车前就马上开始建立 TCP 链接甚至渲染了。接着是输入 URL 后的「回车」，这时浏览器会对 URL 进行检查，首先判断协议，如果是 http 就按照 Web 来处理，另外还会对这个 URL 进行安全检查，然后直接调用浏览器内核中的对应方法
	
* HTTP 请求的发送

	* DNS 查询

		首先由本机所设置的 DNS 服务器(8.8.8.8)向 DNS 根节点查询负责 .com 区域的域务器，然后通过其中一个负责 .com 的服务器查询负责 baidu.com 的服务器，最后由其中一个 baidu.com 的域名服务器查询 fex.baidu.com 域名的地址
		
	* 通过 Socket 发送数据

		可以选择 TCP 或 UDP 协议，HTTP 常用的是 TCP 协议
		
* 服务器接收到数据

	* 负载均衡

		请求在进入到真正的应用服务器前，可能还会先经过负责负载均衡的机器，它的作用是将请求合理地分配到多个服务器上，同时具备具备防攻击等功能
		
	* LVS

		LVS 的作用是从对外看来只有一个 IP，而实际上这个 IP 后面对应是多台机器，因此也被成为 Virtual IP
		
	* 反向代理
		
		是工作在 HTTP 上的，具体实现可以基于 HAProxy 或 Nginx，因为反向代理能理解 HTTP 协议，所以能做非常多的事情，比如：

		* 进行很多统一处理，比如防攻击策略、防抓取、SSL、gzip、自动性能优化等

		* 应用层的分流策略都能在这里做，比如对 /xx 路径的请求分到 a 服务器，对 /yy 路径的请求分到 b 服务器，或者按照 cookie 进行小流量测试等

		* 缓存，并在后端服务挂掉的时候显示友好的 404 页面

		* 监控后端服务是否异常

	* Web Server 中的处理

		请求经过前面的负载均衡后，将进入到对应服务器上的 Web Server，比如 Apache、Tomcat、Node.JS等调用后端语言来处理HTTP请求
		
* 服务器返回数据后浏览器

	服务端处理完请求后，结果将通过网络发回客户端的浏览器，HTTP 请求返回的 HTML 传递到浏览器后进行解码，外链资源的加载，JavaScript 的执行，渲染等
	
	
参考：[从输入 URL 到页面加载完成的过程中都发生了什么事情？](http://fex.baidu.com/blog/2014/05/what-happen/)

本博客版权归lidee92805和饥人谷所有，转载请注明出处





