## HTTP（HyperText Transfer Protocol，超文本传输协议）

HTTP是一个应用层协议，虽然在2015年已推出HTTP/2版本，并被主要的web浏览器和web服务器支持。但目前使用最广泛的还是HTTP/1.1版本。有关历史请查阅这里。
它的主要特点可概括如下：

- 支持客户/服务器模式。
- 简单快速：客户向服务器请求服务时，只需传送请求方法和路径。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快。
- 灵活：HTTP允许传输任意类型的数据对象。正在传输的类型由Content-Type加以标记。
- 无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
- 无状态：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。为了解决这个问题， Web程序引入了Cookie机制来维护状态。

另外，HTTP请求报文和响应报文都是由开始行（对于请求消息，开始行就是请求行，对于响应消息，开始行就是状态行），消息报头（可选），空行（只有CRLF的行），消息正文（可选）组成。将在下面详细讲解。

### 请求报文结构

报文中的数据都使用ASCII编码，各个字段的长度是不确定的（除了作为结尾的CRLF外，不允许出现单独的CR或LF字符）。

![](/image/4-8-1.png)

### 请求报文样例

```
POST /search HTTP/1.1  

Accept: image/gif, image/x-xbitmap, image/jpeg, image/pjpeg, application/vnd.ms-excel, application/vnd.ms-powerpoint, 
application/msword, application/x-silverlight, application/x-shockwave-flash, */*  

Referer: http://www.google.cn/  
Accept-Language: zh-cn  
Accept-Encoding: gzip, deflate  
User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727; TheWorld)  
Host: www.google.cn 
Connection: Keep-Alive  

Cookie: PREF=ID=80a06da87be9ae3c:U=f7167333e2c3b714:NW=1:TM=1261551909:LM=1261551917:S=ybYcq2wpfefs4V9g; 
NID=31=ojj8d-IygaEtSxLgaJmqSjVhCspkviJrB6omjamNrSm8lZhKy_yMfO2M4QMRKcH1g0iQv9u-2hfBW7bUFwVh7pGaRUb0RnHcJU37y-
FxlRugatx63JLv7CWMD6UB_O_r  

hl=zh-CN&source=hp&q=domety
```

### 请求报文参数详解

#### 请求方法

所有请求方法名称全为大写，目前有9种：

![](/image/4-8-3.png)

备注
安全性：https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol
幂等性：表示的操作至多只会被处理一次，每次调用都将返回第一次调用时的处理结果。

#### 关于HTTP请求GET和POST的区别

1. 提交形式：
GET提交的数据会放在URL之后，以?分割URL和传输数据，参数之间以&相连，如EditPosts.aspx?name=test1&id=123456.  POST方法是把提交的数据放在HTTP包的Body中

2. 传输数据的大小：
HTTP协议本身没有对传输的数据大小进行限制，HTTP协议规范也没有对URL长度进行限制。 而在实际开发中存在的限制主要有：
   GET:特定浏览器和服务器对URL长度有限制，例如IE对URL长度的限制是2083字节(2K+35)。对于其他浏览器，如Netscape、FireFox等，理论上没有长度限制，其限制取决于操作系统的支持。
   因此对于GET提交时，传输数据就会受到URL长度的限制。
   POST:由于不是通过URL传值，理论上数据不受限。但实际各个WEB服务器会规定对post提交数据大小进行限制，Apache、IIS6都有各自的配置。
   
3. 安全性： 
POST的安全性要比GET的安全性高，具有真正的Security的含义。而且通过GET提交数据，用户名和密码将明文出现在URL上，因为登录页面有可能被浏览器缓存，其他用户浏览历史纪录就可以拿到账号和密码了。

#### 请求报头域

报头域指头部中的Key，且不分大小写。

![](/image/4-8-4.png)