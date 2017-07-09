## 跨域问题及其解决

### 产生跨域问题的原因

跨域问题是由于浏览器[同源策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)，同源策略限制从一个源加载的文档或脚本如何与来自另一个源的资源进行交互。这是一个用于隔离潜在恶意文件的关键的安全机制。

#### 一个源的定义:

如果协议，端口（如果指定了一个）和域名对于两个页面是相同的，则两个页面具有相同的源。

如`https://www.github.com:80`,https是协议，80是端口，`www.github.com`是域名。有一个不同，则不同源。

下表给出了相对`http://store.company.com/dir/page.html`同源检测的示例:

| URL           | 结果	         | 原因   |
| ------------- |:-------------:| -----:|
| http://store.company.com/dir2/other.html           | 成功    | dir2/other.html |
| http://store.company.com/dir/inner/another.html    |成功     |   dir/inner/another.html |
|http://store.company.com/dir/inner/another.html     | 失败    |    不同的协议 ( https ) |
|http://store.company.com:81/dir/etc.html            | 失败    |  	不同的端口 ( 81 ) |
|http://news.company.com/dir/other.html	       | 失败    |   不同的域名 ( news ) |

## 解決方法

1、使用[jsonp](jsonp.md)

2、服务端代理

在服务器端设置一个代理，由服务器端向跨域下的网站发出请求，再将请求结果返回给前端，成功避免同源策略的限制。

3、HTTP访问控制[CORS](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)

设置Access-Control-Allow-Origin。