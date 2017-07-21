## 页面呈现、重绘、回流

页面呈现流程 

在讨论页面重绘、回流之前。需要对页面的呈现流程有些了解，页面是怎么把html结合css等显示到浏览器上的，下面的流程图显示了浏览器对页面的呈现的处理流程。可能不同的浏览器略微会有些不同。但基本上都是类似的。
     



1. 浏览器把获取到的html代码解析成1个Dom树，html中的每个tag都是Dom树中的1个节点，根节点就是我们常用的document对象(<html> tag)。dom树就是我们用firebug或者IE Developer Toolbar等工具看到的html结构，里面包含了所有的html tag，包括display:none隐藏，还有用JS动态添加的元素等。

2. 浏览器把所有样式(主要包括css和浏览器的样式设置)解析成样式结构体，在解析的过程中会去掉浏览器不能识别的样式，比如IE会去掉-moz开头的样式，而firefox会去掉_开头的样式。

3. dom tree和样式结构体结合后构建呈现树(render tree),render tree有点类似于dom tree，但其实区别有很大，render tree能识别样式，render tree中每个node都有自己的style，而且render tree不包含隐藏的节点(比如display:none的节点，还有head节点)，因为这些节点不会用于呈现，而且不会影响呈现的，所以就不会包含到render tree中。注意 visibility:hidden隐藏的元素还是会包含到render tree中的，因为visibility:hidden 会影响布局(layout)，会占有空间。根据css2的标准，render tree中的每个节点都称为box(Box dimensions)，box所有属性：width,height,margin,padding,left,top,border等。

4. 一旦render tree构建完毕后，浏览器就可以根据render tree来绘制页面了。