---
layout: post
title: 通过NSURLProtocol实现URL重定向
category: iOS开发
date: 2015-06-09
---
开发项目时,经常需要切换内外网进行调试,目前采用的方法是定义DEBUG_MODE的宏,编译不同的HTTP的header.感觉不太灵活,最好在程序中直接控制发送的网络的URL进行内外网的切换.在看到[阿毛的蛋疼地的文章](http://xiangwangfeng.com/2014/11/29/NSURLProtocol%E5%92%8CNSRunLoop%E7%9A%84%E9%82%A3%E4%BA%9B%E5%9D%91/)知道了NSURLProtocol,于是进行尝试.


要对URL进行重定向,就要在对Request的host进行更改,代码如下


<pre>
+ (BOOL)canInitWithRequest:(NSURLRequest *)request {
    NSLog(@"%@",request.URL);
    if ([NSURLProtocol propertyForKey:@"LDHURLProtocolKey" inRequest:request]) {
        return NO;
    }
    return YES;
}
+ (NSURLRequest *)canonicalRequestForRequest:(NSURLRequest *)request {
    return request;
}
- (void)startLoading {
    NSMutableURLRequest * mutableRequest = [self.request mutableCopy];
    NSURLComponents * URLComponents = [NSURLComponents componentsWithString:[[mutableRequest URL] absoluteString]];
    if ([URLComponents.host isEqualToString:@"www.baidu.com"]) {
        URLComponents.host = @"www.163.com";
    }
    [NSURLProtocol setProperty:@YES forKey:@"LDHURLProtocolKey" inRequest:mutableRequest];
    mutableRequest.URL = [URLComponents URL];
    _connection = [NSURLConnection connectionWithRequest:mutableRequest delegate:self];
}
</pre>


![Alt text](/images/2015年6月9日01.png)


后来又尝试在+ (NSURLRequest *)canonicalRequestForRequest:(NSURLRequest *)request中修改request,修改代码如下</br>
<pre>
+ (NSURLRequest *)canonicalRequestForRequest:(NSURLRequest *)request {
    NSMutableURLRequest * mutableRequest = [request mutableCopy];
    NSURLComponents * URLComponents = [NSURLComponents componentsWithString:[[mutableRequest URL] absoluteString]];
    if ([URLComponents.host isEqualToString:@"www.baidu.com"]) {
        URLComponents.host = @"www.163.com";
    }  
    [NSURLProtocol setProperty:@YES forKey:@"LDHURLProtocolKey" inRequest:mutableRequest];
    mutableRequest.URL = [URLComponents URL];
    return mutableRequest;
//    return request;
}
- (void)startLoading {
    _connection = [NSURLConnection connectionWithRequest:self.request delegate:self];
}
</pre>


在iOS8上运行OK,在iOS7上会循环调用网络请求,不能正常运行,可见在iOS8上苹果优化了 URL Loading System,在参考了[NSEtcHosts](https://github.com/mattt/NSEtcHosts)后
修改startLoading中代码
<pre>
- (void)startLoading {
    //    _connection = [NSURLConnection connectionWithRequest:self.request delegate:self];
        _connection = [NSURLConnection connectionWithRequest:[[self class] canonicalRequestForRequest:self.request] delegate:self];
}
</pre>
在iOS7上运行正常,加个switch控制URLProtocol是否handle request.</br>因为NSURLComponents是iOS7之后出现的类,在iOS6上需要对request的url进行解析出host进行判断,然后创建新的url替换request的url,未进行测试.


[demo](https://github.com/lidee92805/URLRedirection)已经放到github上
