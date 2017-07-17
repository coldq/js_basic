## Js的小技巧

### 匿名函数 自执行

1.什么是自执行的匿名函数?
它是指形如这样的函数: (function {// code})();

2.匿名函数的好处在于：可以减少局部变量，以免污染现有的运行环境。

3.疑问
为什么(function {// code})();可以被执行, 而function {// code}();却会报错?

4.分析
(1). 首先, 要清楚两者的区别:
(function {// code})是表达式, function {// code}是函数声明.

(2). 其次, js"预编译"的特点:
js在"预编译"阶段, 会解释函数声明, 但却会忽略表式.

(3). 当js执行到function() {//code}();时, 由于function() {//code}在"预编译"阶段已经被解释过, js会跳过function(){//code}, 试图去执行();, 故会报错;
当js执行到(function {// code})();时, 由于(function {// code})是表达式, js会去对它求解得到返回值, 由于返回值是一 个函数, 故而遇到();时, 便会被执行.

另外， 函数转换为表达式的方法并不一定要靠分组操作符()，我们还可以用void操作符，~操作符，!操作符,,+,-等一元操作符
如：

```
!function(){ 
alert("另类的匿名函数自执行"); 
}();
```

### 判断是否是Array

#### instanceof 判断

```
var ary = [1,23,4];
console.log(ary instanceof Array)//true;
```

从输出的效果来看，还是挺令人满意的，能准确的检测出数据类型是否是数组，不要高兴的太早，大家先想想这个的缺点，我们接着说第三种方法

#### 原型链方法
```
var ary = [1,23,4];
console.log(ary.__proto__.constructor==Array);//true
console.log(ary.constructor==Array)//true 这两段代码是一样的
```

这个办法开起来好高大上哦~~，利用了原型链的方法，但是但是，这个是有兼容的哦，在IE早期版本里面__proto__是没有定义的哦~而且，这个仍然有局限性，我们现在就来总结一下第2种方法和第3种方法局限性

#### 总结一下第2种方法和第3种方法局限性

instanceof 和constructor 判断的变量，必须在当前页面声明的，比如，一个页面（父页面）有一个框架，框架中引用了一个页面（子页面），在子页面中声明了一个ary，并将其赋值给父页面的一个变量，这时判断该变量，Array == object.constructor;会返回false；
原因：

1、array属于引用型数据，在传递过程中，仅仅是引用地址的传递。

2、**每个页面的Array原生对象所引用的地址是不一样的，在子页面声明的array，所对应的构造函数，是子页面的Array对象**；父页面来进行判断，使用的Array并不等于子页面的Array；切记，不然很难跟踪问题！

#### 通用的方法

```
var ary = [1,23,4];
function isArray(o){
return Object.prototype.toString.call(o)=='[object Array]';
}
console.log(isArray(ary));
```

### 判断Object是否空

Object.getOwnPropertyNames(obj)或Object.keys(obj)返回一个数组，该数组对元素是 obj 自身拥有的枚举或不可枚举属性名称字符串。
所以可以用下面方法判断obj是否为空：

```
Object.getOwnPropertyNames(obj).length === 0 
Object.keys(obj).length === 0
```

注意用`for i in obj `会返回原型链上的属性

```
function Foo(){
}
Foo.prototype.a = 1;

var foo = new Foo();
foo.b = 2;
Object.keys(foo);//["b"]
for(var i in foo){console.log(i)}; //b a
```

其它方法：
```
JSON.stringify(obj) === '{}'
```

### ~~ |0
JavaScript中是没有整型概念的，但利用好位操作符可以轻松处理，同时获得效率上的提升。

|0和~~是很好的一个例子，使用这两者可以将浮点转成整型且效率方面要比同类的parseInt,Math.round 要快。在处理像素及动画位移等效果的时候会很有用。性能比较见此。

```
var foo = (12.4 / 4.13) | 0;//结果为3
var bar = ~~(12.4 / 4.13);//结果为3
```

顺便说句，!!将一个值方便快速转化为布尔值 !!window===true 。

### 变量交换

```
let a=1, b=2;
[a,b] = [b,a]// a:2,b:1