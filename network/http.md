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
NID=31=ojj8d-IygaEtSxLgaJmqSjVhCspkviJrB6omjamNrSm8lZhKy_yMfO2M4QMRKcH1g0iQv9u-2hfBW7bUFwVh7pGaRUb0RnHcJU37y-FxlRugatx63JLv7CWMD6UB_O_r  
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

上图中cache-control的说明有误，正确的是：

- public	所有内容都将被缓存(客户端和代理服务器都可缓存)
- private	内容只缓存到私有缓存中(仅客户端可以缓存，代理服务器不可缓存)
- no-cache	必须先与服务器确认返回的响应是否被更改，然后才能使用该响应来满足后续对同一个网址的请求。因此，如果存在合适的验证令牌 (ETag)，no-cache 会发起往返通信来验证缓存的响应，如果资源未被更改，可以避免下载。
- no-store	所有内容都不会被缓存到缓存或 Internet 临时文件中
- must-revalidation/proxy-revalidation	如果缓存的内容失效，请求必须发送到服务器/代理以进行重新验证
- max-age=xxx (xxx is numeric)	缓存的内容将在 xxx 秒后失效, 这个选项只在HTTP 1.1可用, 并如果和Last-Modified一起使用时, 优先级较高

### 响应报文结构

如所见，响应报文结构与请求报文结构唯一真正的区别在于第一行中用状态信息代替了请求信息。状态行（status line）通过提供一个状态码来说明所请求的资源情况。



![](/image/4-8-5.png)

### 响应报文样例

```html
HTTP/1.1 200 OK
Date: Mon, 23 May 2005 22:38:34 GMT
Content-Type: text/html; charset=UTF-8
Content-Encoding: UTF-8
Content-Length: 138
Last-Modified: Wed, 08 Jan 2003 23:11:55 GMT
Server: Apache/1.3.3.7 (Unix) (Red-Hat/Linux)
ETag: "3f80f-1b6-3e1cb03b"
Accept-Ranges: bytes
Connection: close

<html>
<head>
  <title>An Example Page</title>
</head>
<body>
  Hello World, this is a very simple HTML document.
</body>
</html>
```

### 响应报文参数详解

#### 响应状态码

状态代码由三位数字组成，第一个数字定义了响应的类别，且有五种可能取值。

- 1xx：指示信息--表示请求已接收，继续处理。
- 2xx：成功--表示请求已被成功接收、理解、接受。
- 3xx：重定向--要完成请求必须进行更进一步的操作。
- 4xx：客户端错误--请求有语法错误或请求无法实现。
- 5xx：服务器端错误--服务器未能实现合法的请求。

#### 常用状态码：

200 OK：成功返回状态，对应，GET,PUT,PATCH,DELETE。

201 created  - 成功创建。

302 Found：重定向，新的URL会在response中的Location中返回，浏览器将会使用新的URL发出新的Request。
例如在IE中输入http://www.google.com. HTTP服务器会返回302， IE取到Response中Location header的新URL, 又重新发送了一 个 Request.

304 Not Modified：代表上次的文档已经被缓存了， 还可以继续使用。

400 bad request   - 请求格式错误。

401 unauthorized   - 未授权。

403 forbidden   - 鉴权成功，但是该用户没有权限。

404 not found - 请求的资源不存在。

405 method not allowed - 该http方法不被允许。

410 gone - 这个url对应的资源现在不可用。

415 unsupported media type - 请求类型错误。

422 unprocessable entity - 校验错误时用。

429 too many request - 请求过多。

500 Internal Server Error：服务器发生了不可预期的错误。

503 Server Unavailable：服务器当前不能处理客户端的请求，一段时间后可能恢复正常。

其它状态码请查阅：https://en.wikipedia.org/wiki/List_of_HTTP_status_codes

#### 响应报头域

报头域指头部中的Key，且不分大小写。

![](/image/4-8-6.png)

### Session和Cookie

说到HTTP，就不得不提Session和Cookie。但严格来说，Session和Cookie并不是http协议的一部分。由于HTTP协议设计原则是无状态的，但是近年来出现了种种需求，其中cookie的作用就是为了解决HTTP协议无状态的缺陷所作出的努力。后来出现的session机制则是又一种在客户端与服务器之间保持状态的解决方案。 具体来说cookie机制采用的是在客户端保持状态的方案，而session机制采用的是在服务器端保持状态的方案。同时我们也看到，由于采用服务器端保持状态的方案在客户端也需要保存一个标识，所以session机制可能需要借助于cookie机制来达到保存标识的目的，但实际上它还有其他选择。

#### Session

Session是可以存储针对于某一个用户的浏览器以及通过其当前窗口打开的任何窗口具有针对性的用户信息存储机制。 
通常大家认为，只要关闭浏览器，session就消失，其实这是错误的理解。对session来说也是一样的，除非程序通知服务器删除一个session，否则服务器会一直保留。由于关闭浏览器不会导致session被删除，迫使服务器为seesion设置了一个失效时间，当距离客户端上一次使用session的时间超过这个失效时间时，服务器就可以认为客户端已经停止了活动，才会把session删除以节省存储空间.

(1)第一次访问某个web站点资源时，客户端提交没有带SessionID的请求（请求报文头没有Cookie头域信息）。
  而web服务器会检查是否有SessionID过来，没有则创建SessionID，并根据web程序自身定义在请求哪个资源时添加属于当前会话的信息（也可为空），这个信息列表以SessionID作为标识。然后将SessionID返回给客户端（通过响应报文头的Set-Cookie头域）。
(2 )客户端再次访问同个web站点时，提交带有SessionID的请求（通过Cookie头域存储SessionID）。由服务端判断session是否失效，如果未失效，可查询属于当前会话的信息列表。如果失效，则创建新的session（产生新的SessionID），而原先的session（包含session带的信息列表）则丢失，无法访问。

![](/image/4-8-7.png)

![](/image/4-8-8.png)


#### Cookie

保存SessionID的方式可以采用Cookie，这样在交互过程中浏览器可以自动的按照规则把这个SessionID发回给服务器。Cookie的命名方式类似于SessionID。有时Cookie被人为的禁止，所以出现了其他机制以便在Cookie被禁止时仍然能够把SessionID传递回服务器。这种技术叫做URL重写，就是把SessionID直接附加在URL路径的后面，附加方式也有两种，一种是作为URL路径的附加信息，表现形式为`http://www.wantsoft.com/index.asp;jsessionid= ByOK3vjFD75aPnrF7C2HmdnV6QZcEbzWoWiBYEnLerjQ99zWpBng!-145788764 `。
另一种是作为查询字符串附加在URL后面，表现形式为`http://www.wantsoft.com/index?js ... 99zWpBng!-145788764 `。

#### 相关资料


[Hypertext Transfer Protocol](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol)
[HTTP 协议详解](http://kb.cnblogs.com/page/130970/)
[HTTP请求报文和HTTP响应报文](http://blog.csdn.net/zhangliang_571/article/details/23508953)
[HTTP协议详解（经典）](http://blog.csdn.net/gueter/archive/2007/03/08/1524447.aspx)
[HTTP协议之Session和Cookie](http://leowzy.iteye.com/blog/1216841)