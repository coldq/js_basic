## DOM节点类型

DOM是javascript操作网页的接口，全称为文档对象模型(Document Object Model)。本文将主要说明DOM节点类型，有需要的可以参考借鉴。

### DOM作用
DOM的作用是将网页转为一个javascript对象，从而可以使用javascript对网页进行各种操作(比如增删内容)。浏览器会根据DOM模型，将HTML文档解析成一系列的节点，再由这些节点组成一个树状结构。DOM的最小组成单位叫做节点(node)，文档的树形结构(DOM树)由12种类型的节点组成。


### DOM节点

一般地，节点至少拥有nodeType、nodeName和nodeValue这三个基本属性。
节点类型不同，这三个属性的值也不相同

#### nodeType

| 节点nodeType| 类型 | 描述   |
| ------------- |:-------------:| -----:|
| 1	| Node.ELEMENT_NODE(1)| 元素节点| 
| 2	|   Node.ATTRIBUTE_NODE(2)| 属性节点| 
| 3	| Node.TEXT_NODE(3)| 文本节点,代表元素或属性中的文本内容| 
| 4	| Node.CDATA_SECTION_NODE(4)| CDATA节点,代表文档中的 CDATA 部分（不会由解析器解析的文本）| 
| 5	|  Node.ENTRY_REFERENCE_NODE(5)|  实体引用名称节点   | 
| 6	| Node.ENTITY_NODE(6)| 实体名称节点  | 
| 7	|  Node.PROCESSING_INSTRUCTION_NODE(7)| 处理指令节点| 
| 8	|  Node.COMMENT_NODE(8)| 注释节点 | 
| 9	|  Node.DOCUMENT_NODE(9)| 代表整个文档（DOM 树的根节点）。| 
| 10	| Node.DOCUMENT_TYPE_NODE(10)| 文档类型节点,向为文档定义的实体提供接口| 
| 11	|  Node.DOCUMENT_FRAGMENT_NODE(11) | 文档片段节点,代表轻量级的 Document 对象，能够容纳文档的某个部分| 
| 12	| Node.DOCUMENT_FRAGMENT_NODE(11)| DTD声明节点  | 

DOM定义了一个Node接口，这个接口在javascript中是作为Node类型实现的，而在IE8-浏览器中的所有DOM对象都是以COM对象的形式实现的。所以，IE8-浏览器并不支持Node对象的写法

```
//在标准浏览器下返回1，而在IE8-浏览器中报错，提示Node未定义
console.log(Node.ELEMENT_NODE);//1
```

#### 各种节点详细

##### 1.元素节点

元素节点element对应网页的HTML标签元素。元素节点的节点类型nodeType值是1，节点名称nodeName值是大写的标签名，nodeValue值是null

以body元素为例
```
// 1 'BODY' null
console.log(document.body.nodeType,document.body.nodeName,document.body.nodeValue)
console.log(Node.ELEMENT_NODE === 1);//true
 ```
 
##### 2.特性节点

元素特性节点attribute对应网页中HTML标签的属性，它只存在于元素的attributes属性中，并不是DOM文档树的一部分。特性节点的节点类型nodeType值是2，节点名称nodeName值是属性名，nodeValue值是属性值
现在，div元素有id="test"的属性

```
<div id="test"></div>
<script>
var attr = test.attributes.id;
//2 'id' 'test'
console.log(attr.nodeType,attr.nodeName,attr.nodeValue)
console.log(Node.ATTRIBUTE_NODE === 2);//true 
</script>
```

##### 3.文本节点

文本节点text代表网页中的HTML标签内容。文本节点的节点类型nodeType值是3，节点名称nodeName值是'#text'，nodeValue值是标签内容值

现在，div元素内容为'测试'

```
<div id="test">测试</div>
<script>
var txt = test.firstChild;
//3 '#text' '测试'
console.log(txt.nodeType,txt.nodeName,txt.nodeValue)
console.log(Node.TEXT_NODE === 3);//true 
</script>
 ```
 
##### 4.CDATA节点
CDATASection类型只针对基于XML的文档，只出现在XML文档中，表示的是CDATA区域，格式一般为
```
<![CDATA[
]]>
```

该类型节点的节点类型nodeType的值为4，节点名称nodeName的值为'#cdata-section'，nodevalue的值是CDATA区域中的内容 

##### 5.实体引用名称节点

实体是一个声明，指定了在XML中取代内容或标记而使用的名称。 实体包含两个部分， 首先，必须使用实体声明将名称绑定到替换内容。 实体声明是使用 <!ENTITY name "value"> 语法在文档类型定义(DTD)或XML架构中创建的。其次，在实体声明中定义的名称随后将在 XML 中使用。 在XML中使用时，该名称称为实体引用。
实体引用名称节点entry_reference的节点类型nodeType的值为5，节点名称nodeName的值为实体引用的名称，nodeValue的值为null

```
//实体名称
<!ENTITY publisher "Microsoft Press">
//实体名称引用
<pubinfo>Published by &publisher;</pubinfo>
```

##### 6.实体名称节点

上面已经详细解释过，就不再赘述
该节点的节点类型nodeType的值为6，节点名称nodeName的值为实体名称，nodeValue的值为null

##### 7.处理指令节点

处理指令节点ProcessingInstruction的节点类型nodeType的值为7，节点名称nodeName的值为target，nodeValue的值为entire content excluding the target

##### 8.注释节点
注释节点comment表示网页中的HTML注释。注释节点的节点类型nodeType的值为8，节点名称nodeName的值为'#comment'，nodeValue的值为注释的内容

现在，在id为myDiv的div元素中存在一个<!-- 我是注释内容 -->
```
<div id="myDiv"><!-- 我是注释内容 --></div>
<script>
var com = myDiv.firstChild;
//8 '#comment' '我是注释内容'
console.log(com.nodeType,com.nodeName,com.nodeValue)
console.log(Node.COMMENT_NODE === 8);//true 
</script>
```

##### 9.文档节点
文档节点document表示HTML文档，也称为根节点，指向document对象。文档节点的节点类型nodeType的值为9，节点名称nodeName的值为'#document'，nodeValue的值为null

```
<script>
//9 "#document" null
console.log(document.nodeType,document.nodeName,document.nodeValue)
console.log(Node.DOCUMENT_NODE === 9);//true 
</script>
 ```
 
##### 10.文档类型节点

文档类型节点DocumentType包含着与文档的doctype有关的所有信息。文档类型节点的节点类型nodeType的值为10，节点名称nodeName的值为doctype的名称，nodeValue的值为null

```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Document</title>
</head>
<body>
<script>
var nodeDocumentType = document.firstChild;
//10 "html" null
console.log(nodeDocumentType.nodeType,nodeDocumentType.nodeName,nodeDocumentType.nodeValue);
console.log(Node.DOCUMENT_TYPE_NODE === 10);
</script>
</body>
</html>
```

##### 11.文档片段节点

文档片段节点DocumentFragment在文档中没有对应的标记，是一种轻量级的文档，可以包含和控制节点，但不会像完整的文档寻亲战胜额外的资源。该节点的节点类型nodeType的值为11，节点名称nodeName的值为'#document-fragment'，nodeValue的值为null

```
<script>
var nodeDocumentFragment = document.createDocumentFragment(); 
//11 "#document-fragment" null
console.log(nodeDocumentFragment.nodeType,nodeDocumentFragment.nodeName,nodeDocumentFragment.nodeValue);
console.log(Node.DOCUMENT_FRAGMENT_NODE === 11);//true
</script>
```

##### 12.DTD声明节点

DTD声明节点notation代表DTD中声明的符号。该节点的节点类型nodeType的值为12，节点名称nodeName的值为符号名称，nodeValue的值为null 

#### 节点关系

![](/image/3-1-1.jpg)

##### 节点方法：

| 方法 | 描述   |
| ------------- |:-------------:| -----:|
|nodeType	|返回节点类型的数字值（1~12）|
|nodeName	|元素节点:标签名称(大写)、属性节点:属性名称、文本节点:#text、文档节点:#document|
|nodeValue	|文本节点:包含文本、属性节点:包含属性、元素节点和文档节点:null|
|parentNode	|父节点|
|parentElement	|父节点标签元素|
|childNodes	|所有子节点|
|children	|第一层子节点|
|firstChild	|第一个子节点，Node 对象形式|
|firstElementChild	|第一个子标签元素|
|lastChild	|最后一个子节点|
|lastElementChild	|最后一个子标签元素|
|previousSibling	|上一个兄弟节点|
|previousElementSibling	 |上一个兄弟标签元素|
|nextSibling	|下一个兄弟节点|
|nextElementSibling	|下一个兄弟标签元素|
|childElementCount	|第一层子元素的个数(不包括文本节点和注释)|
|ownerDocument	|指向整个文档的文档节点|

Eg:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <div id="t">
        <span></span>
        <span id="s">
            <a></a>
            <h1>Nick</h1>
        </span>
        <p></p>
    </div>
    <script>
        var tT = document.getElementById("t");
        console.log(tT.nodeType,tT.nodeName,tT.nodeValue); //1 "DIV" null
        console.log(tT.parentNode); //<body>...</body>
        console.log(tT.childNodes); //[text, span, text, span#s, text, p, text]
        console.log(tT.children); //[span, span#s, p, s: span#s]

        var sT = document.getElementById("s");
        console.log(sT.previousSibling); //#text, Node 对象形式
        console.log(sT.previousElementSibling); //<span></span>
        console.log(sT.nextSibling); //#text
        console.log(sT.nextElementSibling); //<p></p>
        console.log(sT.firstChild); //#text
        console.log(sT.firstElementChild); //<a></a>
        console.log(sT.lastChild); //#text
        console.log(sT.lastElementChild); //<h1>Nick</h1>

        console.log(tT.childElementCount); //3
        console.log(tT.ownerDocument); //#document

    </script>
</body>
</html>
```

##### 节点关系方法：

hasChildNodes()  包含一个或多个节点时返回true

contains()  如果是后代节点返回true

isSameNode()、isEqualNode()  传入节点与引用节点的引用为同一个对象返回true

compareDocumentPostion()  确定节点之间的各种关系:

```
数值     关系 
1      给定节点不在当前文档中
2      给定节点位于参考节点之前
4      给定节点位于参考节点之后
8      给定节点包含参考节点
16    给定节点被参考节点包含
```

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <div id="t">
        <span></span>
        <span id="s">
            <a></a>
            <h1>Nick</h1>
        </span>
        <p></p>
    </div>
    <script>
        var tT = document.getElementById("t");
        var sT = document.getElementById("s");


        console.log(tT.hasChildNodes()); //true
        console.log(tT.contains(document.getElementById('s'))); //true
        console.log(tT.compareDocumentPosition(document.getElementById('s'))); //20,因为s被tT包含，所以为16；而又在tT之后,所以为4，两者相加为20.
        console.log(tT.isSameNode(document.getElementById('t'))); //true
        console.log(tT.isEqualNode(document.getElementById('t'))); //true
        console.log(tT.isSameNode(document.getElementById('s'))); //false
    </script>
</body>
</html>
```
