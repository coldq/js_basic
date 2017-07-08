# Js中this的相关知识

## this绑定机制

#### 1. 默认绑定

首先介绍的是函数调用类型：独立函数调用，在没有其他应用下的默认规则。这时候this指向了window。

```
function foo(){
  var a = 2;
  function bar(){
    console.log(this.a)
  }
  bar();
}
foo();// undefined
```

这段代码中,foo函数中的bar函数运行时，由于默认绑定，this指向window，a未定义导致输出为undefined。

#### 2. 隐式绑定

另一条规则是调用的位置是否有上下文对象，或者说是否被某个对象拥有或者包含，不过这种说法可能会造成一些误导。

```
function foo(){
    console.log(this.a);
}
var obj = {
    a:2,
    foo:foo
}
var objContainer = {
    a:12,
    obj:obj
}
objContainer.obj.foo();//2
```

首先需要注意的是foo()的声明方式，及其之后是如何被当作引用属性添加到obj中的，但是无论是直接在obj中定义还是先定义再添加引用属性，这个函数严格来说都不属于obj对象。 

然而，调用位置会使用obj上下文来引用函数，因此你可以说函数被调用时obj对象“拥有”或者“包含”它。
 
无论你如何称呼，当foo()被调用时，它的前面确实加上了对obj的引用，隐式绑定规则会把函数调用中的this绑定到这个上下文对象。 
对象属性引用链只有上一层或者说最后一层在调用中起作用。

