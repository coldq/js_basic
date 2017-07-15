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
