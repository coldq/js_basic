## css引入方式


### 内联方式

内联方式指的是直接在 HTML 标签中的 style 属性中添加 CSS。

示例：

```
<div style="background: red"></div>
```

这通常是个很糟糕的书写方式，它只能改变当前标签的样式，如果想要多个 <div> 拥有相同的样式，你不得不重复地为每个 <div> 添加相同的样式，如果想要修改一种样式，又不得不修改所有的 style 中的代码。很显然，内联方式引入 CSS 代码会导致 HTML 代码变得冗长，且使得网页难以维护。

### 嵌入方式

嵌入方式指的是在 HTML 头部中的 <style> 标签下书写 CSS 代码。

示例：
```
<head>
    <style>

    .content {
        background: red;
    }

    </style>
</head>
```

嵌入方式的 CSS 只对当前的网页有效。因为 CSS 代码是在 HTML 文件中，所以会使得代码比较集中，当我们写模板网页时这通常比较有利。因为查看模板代码的人可以一目了然地查看 HTML 结构和 CSS 样式。因为嵌入的 CSS 只对当前页面有效，所以当多个页面需要引入相同的 CSS 代码时，这样写会导致代码冗余，也不利于维护。

### 链接方式

链接方式指的是使用 HTML 头部的 <head> 标签引入外部的 CSS 文件。

示例：
```
<head>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
```

这是最常见的也是最推荐的引入 CSS 的方式。使用这种方式，所有的 CSS 代码只存在于单独的 CSS 文件中，所以具有良好的可维护性。并且所有的 CSS 代码只存在于 CSS 文件中，CSS 文件会在第一次加载时引入，以后切换页面时只需加载 HTML 文件即可。

另外link还有其他的一些用途，比如引入图标

```
<link rel="shortcut icon" href="/favicon.ico" type="image/x-icon" />
```

### 导入方式

导入方式指的是使用 CSS 规则引入外部 CSS 文件。

示例：

```
@import 'style.css' //Windows IE4/ NS4, Mac OS X IE5, Macintosh IE4/IE5/NS4不识别
@import "style.css" //Windows IE4/ NS4, Macintosh IE4/NS4不识别
@import url(style.css) //Windows NS4, Macintosh NS4不识别
@import url('style.css') //Windows NS4, Mac OS X IE5, Macintosh IE4/IE5/NS4不识别
@import url("style.css") //Windows NS4, Macintosh NS4不识别

其中，@import url(style.css) 和@import url("style.css")是最优的选择，兼容的浏览器最多。从字节优化的角度来看@import url(style.css)最值得推荐。
```

两者都是外部引用CSS的方式，但是存在一定的区别：

1. link是XHTML标签，除了加载CSS外，还可以定义RSS等其他事务；@import属于CSS范畴，只能加载CSS。

2. link引用CSS时，在页面载入时同时加载；@import需要页面网页完全载入以后加载。

3. link是XHTML标签，无兼容问题；@import是在CSS2.1提出的，低版本的浏览器不支持。

4. link支持使用Javascript控制DOM去改变样式；而@import不支持。

5. link方式样式的权重高于@import的。