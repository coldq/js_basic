## 事件和冒泡

DOM事件被发送以通知代码已发生的有趣的事情。每个事件都由基于Event 接口的一个对象表示，并且可以具有用于获得关于发生了什么的附加信息的附加自定义字段 and/or 函数。事件可以表示从基本用户交互到渲染模型中发生的事件的自动通知的所有内容。

###　事件类型
[事件类型一览表](https://developer.mozilla.org/zh-CN/docs/Web/Events)


### 冒泡和捕获
冒泡型事件：
事件按照从最特定的事件目标到最不特定的事件目标(document对象)的顺序触发。

IE 5.5: div -> body -> document
IE 6.0: div -> body -> html -> document
Mozilla 1.0: div -> body -> html -> document -> window

捕获型事件(event capturing)：事件从最不精确的对象(document 对象)开始触发，然后到最精确(也可以在窗口级别捕获事件，不过必须由开发人员特别指定)。

DOM事件流：同时支持两种事件模型：捕获型事件和冒泡型事件，但是，捕获型事件先发生。两种事件流会触及DOM中的所有对象，从document对象开始，也在document对象结束。

DOM事件模型最独特的性质是，文本节点也触发事件(在IE中不会)。
支持W3C标准的浏览器在添加事件时用addEventListener(event,fn,useCapture)方法，基中第3个参数useCapture是一个Boolean值，用来设置事件是在事件捕获时执行，还是事件冒泡时执行。而不兼容W3C的浏览器(IE)用attachEvent()方法，此方法没有相关设置，不过IE的事件模型默认是在事件冒泡时执行的，也就是在useCapture等于false的时候执行，所以把在处理事件时把useCapture设置为false是比较安全，也实现兼容浏览器的效果。

### 事件的传播是可以阻止的：
• 在W3c中，使用stopPropagation（）方法
• 在IE下设置cancelBubble = true；

### 事件添加方法



#### Event Listeners
(addEventListener 以及 IE 的 attachEvent)

IE 8 以及更低版本的 IE 中，需要用 attachEvent 方法：
element.attachEvent('onclick', function() { /* do stuff here*/ });
对于 IE 9 和更高版本的 IE，以及其它浏览器，则要用 addEventListener 方法：

```
element.addEventListener('click', function() { /* do stuff here*/ }, false);
```

用上面这种方法（DOM level 2 events），理论上可以为一个元素绑定无数个事件，实际应用中则决定于客户端的电脑内存以及浏览器。

上面的例子应用了匿名函数这个特性，还可以使用构造函数或者闭包来添加事件监听器：

```
var myFunctionReference = function() { /* do stuff here*/ }

element.attachEvent('onclick', myFunctionReference); //detachEvent(event,function); 
element.addEventListener('click', myFunctionReference , false); //removeEventListener(event,function,capture/bubble); 
```

另一个重要特性，则是上面这段代码中最后一行的最后一个参数，用来控制监听器对于冒泡事件的响应。95%的使用场景中，这个参数都为 false，addEventListener 以及内联事件则都没有可以实现相同功能的这个参数。

#### Inline Events 内联事件
(HTML 的 onclick="" 属性，以及 element.onclick)

在所有支持 JavaScript 的浏览器中，可以用下面的方式来添加内联的事件监听器。

```
<a id="testing" href="#" onclick="alert('did stuff inline');">Click me</a>
```

虽然很多有经验的开发人员对这种方式嗤之以鼻，但是它的确足够的简单粗暴。在这里你不能使用闭包或者匿名函数，并且控制域也是有限的。

还有另一种方法：

```
element.onclick = function () { /*do stuff here */ };
```

这个方法能实现相同的效果，并且有更多的控制域（因为是 JS 脚本，而不是 HTML 代码），当然了，也能使用匿名函数、构造函数、闭包。

内联事件一个明显的不足：由于内联事件是作为元素属性保存起来的，这些属性可以被覆盖，所以如果为同一个事件绑定了多个处理程序，那么最后一个处理程序会覆盖之前的程序（多谢 @谦龙 指出此处的翻译错误）。

```
var element = document.getElementById('testing');
element.onclick = function () { alert('did stuff #1'); };
element.onclick = function () { alert('did stuff #2'); };
```

运行上面的示例，将只会看到“did stuff #2”——第二行代码覆盖了默认的内联 onclick 属性，第三行代码又将第二行代码覆盖了，所以会产生这样的结果。


### 事件委托

利用冒泡的原理，把事件加到父级上，触发执行效果。

一般来说，dom需要有事件处理程序，我们都会直接给它设事件处理程序就好了，那如果是很多的dom需要添加事件处理呢？比如我们有100个li，每个li都有相同的click点击事件，可能我们会用for循环的方法，来遍历所有的li，然后给它们添加事件，那这么做会存在什么影响呢？

在JavaScript中，添加到页面上的事件处理程序数量将直接关系到页面的整体运行性能，因为需要不断的与dom节点进行交互，访问dom的次数越多，引起浏览器重绘与重排的次数也就越多，就会延长整个页面的交互就绪时间，这就是为什么性能优化的主要思想之一就是减少DOM操作的原因；如果要用事件委托，就会将所有的操作放到js程序里面，与dom的操作就只需要交互一次，这样就能大大的减少与dom的交互次数，提高性能；

每个函数都是一个对象，是对象就会占用内存，对象越多，内存占用率就越大，自然性能就越差了（内存不够用，是硬伤，哈哈），比如上面的100个li，就要占用100个内存空间，如果是1000个，10000个呢，那只能说呵呵了，如果用事件委托，那么我们就可以只对它的父级（如果只有一个父级）这一个对象进行操作，这样我们就需要一个内存空间就够了，是不是省了很多，自然性能就会更好。

#### 实现

一个ul标签里面有多个li,点击每个li的时候，弹出“li”

```
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
</ul>
<script>
    var li=document.getElementsByTagName('li');
    for(var i=0;i<li.length;i++){
       li[i].onclick=function(){
           alert('li');
       }
    }
 </script>
 ```
   
没错，我们应该是这样写的呀，试试也证明这样确实每点击一个li弹出“li”, 
但是，首先要找到ul，然后遍历li，然后点击li的时候，又要找一次目标的li的位置，才能执行最后的操作，每次点击都要找一次li；浪费时间内存，

那么我们看看用事件委托如何实现：

```
var ul=document.getElementsByTagName('ul')[0];
ul.onclick=function(){
    alert('li');
}
```

这里用父元素ul来做事件处理，当li被点击时，由于冒泡原理，事件就会冒泡到ul上，因为ul上有点击事件，所以事件就会触发，当然，这里当点击ul的时候，也是会触发的。

那么，如果我只想让点击li时才出发事件，点击ul时不触发，怎么办，，么办，办。。

别怕，每个事件都有事件源，我们可以通过事件源的获取来判断是ul呢还是li,那么事件源怎么获取： 
事件对象（Event对象）提供了一个属性叫target，捕获真正被点击的节点元素，

注意： 
不管在哪个事件中，只要你操作的那个元素就是事件源。 
ie下：window.event.srcElement 
标准下:event.target 
nodeName:找到事件源元素的标签名

这样就简单了，，我们可以通过给父元素添加事件监听器。当有事件触发监听器时，然后检查事件的来源，排除非li子元素事件。如果是一个li元素，我们就找到了目标！如果不是一个li元素，事件将被忽略：代码如下：

```
var ul=document.getElementsByTagName('ul')[0];
ul.addEventListener('click',function(ev){
    var ev = event||window.event;
    var target = ev.target || ev.srcElement;
    if(target.nodeName.toLowerCase()=='li'){
       alert('li');
    }
},false)
    
```

用事件委托的方式，新添加的子元素是带有事件效果的。

当用事件委托的时候，根本就不需要去遍历元素的子节点，只需要给父级元素添加事件就好了，其他的都是在js里面的执行，这样可以大大的减少dom操作，这才是事件委托的精髓所在。