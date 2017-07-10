## AJAX如何实现

[参考](http://caibaojian.com/ajax-jsonp.html)

ajax的技术核心是 XMLHttpRequest 对象；

ajax 请求过程：创建 XMLHttpRequest 对象、连接服务器、发送请求、接收响应数据；

```
  ajax({
        url: "./TestXHR.aspx",              //请求地址
        type: "POST",                       //请求方式
        data: { name: "super", age: 20 },        //请求参数
        dataType: "json",
        success: function (response, xml) {
            // 此处放成功后执行的代码
        },
        fail: function (status) {
            // 此处放失败后执行的代码
        }
    });

    function ajax(options) {
        options = options || {};
        options.type = (options.type || "GET").toUpperCase();
        options.dataType = options.dataType || "json";
        var params = formatParams(options.data);

        //创建 - 非IE6 - 第一步
        if (window.XMLHttpRequest) {
            var xhr = new XMLHttpRequest();
        } else { //IE6及其以下版本浏览器
            var xhr = new ActiveXObject('Microsoft.XMLHTTP');
        }

        //接收 - 第三步
        xhr.onreadystatechange = function () {
            if (xhr.readyState == 4) {
                var status = xhr.status;
                if (status >= 200 && status < 300) {
                    options.success && options.success(xhr.responseText, xhr.responseXML);
                } else {
                    options.fail && options.fail(status);
                }
            }
        }

        //连接 和 发送 - 第二步
        if (options.type == "GET") {
            xhr.open("GET", options.url + "?" + params, true);
            xhr.send(null);
        } else if (options.type == "POST") {
            xhr.open("POST", options.url, true);
            //设置表单提交时的内容类型
            xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
            xhr.send(params);
        }
    }
    //格式化参数
    function formatParams(data) {
        var arr = [];
        for (var name in data) {
            arr.push(encodeURIComponent(name) + "=" + encodeURIComponent(data[name]));
        }
        arr.push(("v=" + Math.random()).replace(".",""));
        return arr.join("&");
    }
```


### 1、创建

1.1、IE7及其以上版本中支持原生的 XHR 对象，因此可以直接用： var oAjax = new XMLHttpRequest();

1.2、IE6及其之前的版本中，XHR对象是通过MSXML库中的一个ActiveX对象实现的。有的书中细化了IE中此类对象的三种不同版本，即MSXML2.XMLHttp、MSXML2.XMLHttp.3.0 和 MSXML2.XMLHttp.6.0；个人感觉太麻烦，可以直接使用下面的语句创建： var oAjax=new ActiveXObject(’Microsoft.XMLHTTP’);

### 2、连接和发送

2.1、open()函数的三个参数：请求方式、请求地址、是否异步请求(同步请求的情况极少，至今还没用到过)；

2.2、GET 请求方式是通过URL参数将数据提交到服务器的，POST则是通过将数据作为 send 的参数提交到服务器；

2.3、POST 请求中，在发送数据之前，要设置表单提交的内容类型；

2.4、提交到服务器的参数必须经过 encodeURIComponent() 方法进行编码，实际上在参数列表”key=value”的形式中，key 和 value 都需要进行编码，因为会包含特殊字符。每次请求的时候都会在参数列表中拼入一个 “v=xx” 的字符串，这样是为了拒绝缓存，每次都直接请求到服务器上。

encodeURI() ：用于整个 URI 的编码，不会对本身属于 URI 的特殊字符进行编码，如冒号、正斜杠、问号和井号；其对应的解码函数 decodeURI()；
encodeURIComponent() ：用于对 URI 中的某一部分进行编码，会对它发现的任何非标准字符进行编码；其对应的解码函数 decodeURIComponent()；

### 3、接收

3.1、接收到响应后，响应的数据会自动填充XHR对象，相关属性如下
responseText：响应返回的主体内容，为字符串类型；
responseXML：如果响应的内容类型是 "text/xml" 或 "application/xml"，这个属性中将保存着相应的xml 数据，是 XML 对应的 document 类型；
status：响应的HTTP状态码；
statusText：HTTP状态的说明；

3.2、XHR对象的readyState属性表示请求/响应过程的当前活动阶段，这个属性的值如下
0-未初始化，尚未调用open()方法；
1-启动，调用了open()方法，未调用send()方法；
2-发送，已经调用了send()方法，未接收到响应；
3-接收，已经接收到部分响应数据；
4-完成，已经接收到全部响应数据；

只要 readyState 的值变化，就会调用 readystatechange 事件，(其实为了逻辑上通顺，可以把readystatechange放到send之后，因为send时请求服务器，会进行网络通信，需要时间，在send之后指定readystatechange事件处理程序也是可以的，我一般都是这样用，但为了规范和跨浏览器兼容性，还是在open之前进行指定吧)。

3.3、在readystatechange事件中，先判断响应是否接收完成，然后判断服务器是否成功处理请求，xhr.status 是状态码，状态码以2开头的都是成功，304表示从缓存中获取，上面的代码在每次请求的时候都加入了随机数，所以不会从缓存中取值，故该状态不需判断。

### 4、ajax请求是不能跨域的！
