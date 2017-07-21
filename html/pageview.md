## 页面呈现、重绘、回流

### 页面呈现流程 

在讨论页面重绘、回流之前。需要对页面的呈现流程有些了解，页面是怎么把html结合css等显示到浏览器上的，下面的流程图显示了浏览器对页面的呈现的处理流程。可能不同的浏览器略微会有些不同。但基本上都是类似的。
     
![](/image/3-11-1.jpg)


1. 浏览器把获取到的html代码解析成1个Dom树，html中的每个tag都是Dom树中的1个节点，根节点就是我们常用的document对象(<html> tag)。dom树就是我们用firebug或者IE Developer Toolbar等工具看到的html结构，里面包含了所有的html tag，包括display:none隐藏，还有用JS动态添加的元素等。

2. 浏览器把所有样式(主要包括css和浏览器的样式设置)解析成样式结构体，在解析的过程中会去掉浏览器不能识别的样式，比如IE会去掉-moz开头的样式，而firefox会去掉_开头的样式。

3. dom tree和样式结构体结合后构建呈现树(render tree),render tree有点类似于dom tree，但其实区别有很大，render tree能识别样式，render tree中每个node都有自己的style，而且render tree不包含隐藏的节点(比如display:none的节点，还有head节点)，因为这些节点不会用于呈现，而且不会影响呈现的，所以就不会包含到render tree中。注意 visibility:hidden隐藏的元素还是会包含到render tree中的，因为visibility:hidden 会影响布局(layout)，会占有空间。根据css2的标准，render tree中的每个节点都称为box(Box dimensions)，box所有属性：width,height,margin,padding,left,top,border等。

4. 一旦render tree构建完毕后，浏览器就可以根据render tree来绘制页面了。

### 回流与重绘

1. 当render tree中的一部分(或全部)因为元素的规模尺寸，布局，隐藏等改变而需要重新构建。这就称为回流(其实我觉得叫重新布局更简单明了些)。每个页面至少需要一次回流，就是在页面第一次加载的时候。

2. 当render tree中的一些元素需要更新属性，而这些属性只是影响元素的外观，风格，而不会影响布局的，比如background-color。则就叫称为重绘。

注：从上面可以看出，回流必将引起重绘，而重绘不一定会引起回流。


### 什么操作会引起重绘、回流
   
其实任何对render tree中元素的操作都会引起回流或者重绘，比如：

1. 添加、删除元素(回流+重绘)

2. 隐藏元素，display:none(回流+重绘)，visibility:hidden(只重绘，不回流)

3. 移动元素，比如改变top,left(jquery的animate方法就是,改变top,left不一定会影响回流)，或者移动元素到另外1个父元素中。(重绘+回流)

4. 对style的操作(对不同的属性操作，影响不一样)

5. 还有一种是用户的操作，比如改变浏览器大小，改变浏览器的字体大小等(回流+重绘)