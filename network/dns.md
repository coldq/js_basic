## 前端优化:DNS预解析提升页面速度
在网页体验中我们常会遇到这种情况，即在调用百度联盟、谷歌联盟以及当前网页所在域名外的域名文件时会遇到请求延时非常严重的情况。那么有没有方法去解决这种请求严重延时的现象呢？

一般来说这种延时的原因不会是对方网站带宽或者负载的原因，那么到底是什么导致了这种情况呢。假设是DNS的问题，因为DNS解析速度很可能是造成资源延时的最大原因。于是在页面header中添加了以下代码（用以DNS预解析）：

```
<meta http-equiv="x-dns-prefetch-control" content="on" />
<link rel="dns-prefetch" href="http://bdimg.share.baidu.com" />
<link rel="dns-prefetch" href="http://nsclick.baidu.com" />
<link rel="dns-prefetch" href="http://hm.baidu.com" />
<link rel="dns-prefetch" href="http://eiv.baidu.com" />
```

效果很不错（测试浏览器为IE8），再打开其他页面时百度分享按钮的加载明显提高！

下面我们来简单了解一下dns-prefetch：

DNS 作为互联网的基础协议，其解析的速度似乎容易被网站优化人员忽视。现在大多数新浏览器已经针对DNS解析进行了优化，典型的一次DNS解析耗费20-120 毫秒，减少DNS解析时间和次数是个很好的优化方式。DNS Prefetching是具有此属性的域名不需要用户点击链接就在后台解析，而域名解析和内容载入是串行的网络操作，所以这个方式能减少用户的等待时间，提升用户体验。
浏览器对网站第一次的域名DNS解析查找流程依次为：

浏览器缓存-系统缓存-路由器缓存-ISP DNS缓存-递归搜索


Chrome内置了DNS Prefetching技术, Firefox 3.5 也引入了这一特性，由于Chrome和Firefox 3.5本身对DNS预解析做了相应优化设置，所以设置DNS预解析的不良影响之一就是可能会降低Google Chrome浏览器及火狐Firefox 3.5浏览器的用户体验。

#### Chrome Predictor 的预测功能优化

Chrome 会随着使用变得更快，它这个特性是通过一个单例对象 Predictor 来实现的。这个对象在浏览器内核进程(Browser Kernel Process)中实例化，它唯一的职责就是观察和学习当前网络活动方式，提前预估用户下一步的操作。下面是一个示例：

用户将鼠标停留在一个链接上，就预示着一个用户的偏好以及下一步的浏览行为。这时 Chrome 就可以提前进行 DNS Lookup 及 TCP 握手。用户的点击操作平均需要将近 200ms,在这个时间就可能处理完 DNS 和 TCP 相关的操作， 也就是省去几百毫秒的延迟时间。 当在地址栏(Omnibox/URL bar) 触发高可能性选项时，就同样会触发一个 DNS lookup 和 TCP 预连接(pre-connect)，甚至在一个不可见的页签中进行预渲染(pre-render)！

我们每个人都会有天天访问的网站，Chrome 会研究在这些页面上的子资源， 并且尝试进行预解析(pre-resolve)，甚至可能会进行预加载(pre-fetch)以优化浏览体验。

####　预解析的实现：

1. 用meta信息来告知浏览器, 当前页面要做DNS预解析:`<meta http-equiv="x-dns-prefetch-control" content="on" />`
2. 在页面header中使用link标签来强制对DNS预解析: `<link rel="dns-prefetch" href="http://bdimg.share.baidu.com" />`
注：dns-prefetch需慎用，多页面重复DNS预解析会增加重复DNS查询次数。

PS：DNS预解析主要是用于网站前端页面优化，在SEO中的作用湛蓝还未作验证，但作为增强用户体验的一部分rel="dns-prefetch"或许值得大家慢慢发现

### DNS负载均衡

互联网用户巨大的今天，假如亿万请求请求的资源都位于同一台机器上面，那么这台机器随时可能会蹦掉。处理办法就是用DNS负载均衡技术，它的原理是在DNS服务器中为同一个主机名配置多个IP地址,在应答DNS查询时,DNS服务器对每个查询将以DNS文件中主机记录的IP地址按顺序返回不同的解析结果,将客户端的访问引导到不同的机器上去,使得不同的客户端访问不同的服务器,从而达到负载均衡的目的｡例如可以根据每台机器的负载量，该机器离用户地理位置的距离等等。 　

大家耳熟能详的CDN(Content Delivery Network)就是利用DNS的重定向技术，DNS服务器会返回一个跟用户最接近的点的IP地址给用户，CDN节点的服务器负责响应用户的请求，提供所需的内容。