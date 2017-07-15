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
