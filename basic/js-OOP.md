## 面向对象(OOP)的Js编程

JavaScript 的核心是支持面向对象的，同时它也提供了强大灵活的 OOP 语言能力。本文从对面向对象编程的介绍开始，带您探索 JavaScript 的对象模型，最后描述 JavaScript 当中面向对象编程的一些概念。

### JavaScript回顾
如果您对 JavaScript 的概念（如变量、类型、方法和作用域等）缺乏自信，您可以在重新介绍 JavaScript 这篇文章里学习这些概念。您也可以查阅这篇 [JavaScript 1.5 核心指南](https://developer.mozilla.org/zh-CN/JavaScript/Guide)。

### 面向对象编程
面向对象编程是用抽象方式创建基于现实世界模型的一种编程模式。它使用先前建立的范例，包括模块化，多态和封装几种技术。今天，许多流行的编程语言（如Java，JavaScript，C＃，C+ +，Python，PHP，Ruby和Objective-C）都支持面向对象编程（OOP）。

相对于 “一个程序只是一些函数的集合，或简单的计算机指令列表。”的传统软件设计观念而言，面向对象编程可以看作是使用一系列对象相互协作的软件设计。 在 OOP 中，每个对象能够接收消息，处理数据和发送消息给其他对象。每个对象都可以被看作是一个拥有清晰角色或责任的独立小机器。

面向对象程序设计的目的是在编程中促进更好的灵活性和可维护性，在大型软件工程中广为流行。凭借其对模块化的重视，面向对象的代码开发更简单，更容易理解，相比非模块化编程方法 1, 它能更直接地分析, 编码和理解复杂的情况和过程。

### 术语
#### Namespace 命名空间
允许开发人员在一个独特, 应用相关的名字的名称下捆绑所有功能的容器。
#### Class 类
定义对象的特征。它是对象的属性和方法的模板定义.
#### Object 对象
类的一个实例。
#### Property 属性
对象的特征，比如颜色。
#### Method 方法
对象的能力，比如行走。
#### Constructor 构造函数
对象初始化的瞬间, 被调用的方法. 通常它的名字与包含它的类一致.
#### Inheritance 继承
一个类可以继承另一个类的特征。
#### Encapsulation 封装
一种把数据和相关的方法绑定在一起使用的方法.
#### Abstraction 抽象
结合复杂的继承，方法，属性的对象能够模拟现实的模型。
#### Polymorphism 多态
多意为‘许多’，态意为‘形态’。不同类可以定义相同的方法或属性。
更多关于面向对象编程的描述，请参照维基百科的 [面向对象编程](http://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1%E2%80%8B) 。

### JavaScript面向对象编程
#### 命名空间

命名空间是一个容器，它允许开发人员在一个独特的，特定于应用程序的名称下捆绑所有的功能。 在JavaScript中，命名空间只是另一个包含方法，属性，对象的对象。

注意：需要认识到重要的一点是：与其他面向对象编程语言不同的是，Javascript中的普通对象和命名空间在语言层面上没有区别。这点可能会让JavaScript初学者感到迷惑。
创造的JavaScript命名空间背后的想法很简单：一个全局对象被创建，所有的变量，方法和功能成为该对象的属性。使用命名空间也最大程度地减少应用程序的名称冲突的可能性。

我们来创建一个全局变量叫做 MYAPP

```
// 全局命名空间
var MYAPP = MYAPP || {};
```

在上面的代码示例中，我们首先检查MYAPP是否已经被定义（是否在同一文件中或在另一文件）。如果是的话，那么使用现有的MYAPP全局对象，否则，创建一个名为MYAPP的空对象用来封装方法，函数，变量和对象。

我们也可以创建子命名空间：

```
// 子命名空间
MYAPP.event = {};
```

下面是用于创建命名空间和添加变量，函数和方法的代码写法：
```
// 给普通方法和属性创建一个叫做MYAPP.commonMethod的容器
MYAPP.commonMethod = {
  regExForName: "", // 定义名字的正则验证
  regExForPhone: "", // 定义电话的正则验证
  validateName: function(name){
    // 对名字name做些操作，你可以通过使用“this.regExForname”
    // 访问regExForName变量
  },
 
  validatePhoneNo: function(phoneNo){
    // 对电话号码做操作
  }
}

// 对象和方法一起申明
MYAPP.event = {
    addListener: function(el, type, fn) {
    //  代码
    },
   removeListener: function(el, type, fn) {
    // 代码
   },
   getEvent: function(e) {
   // 代码
   }
  
   // 还可以添加其他的属性和方法
}

//使用addListener方法的写法:
MYAPP.event.addListener("yourel", "type", callback);
```
