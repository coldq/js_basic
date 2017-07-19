## 关于JavaScript的变量提升
[转](https://github.com/Wscats/Good-text-Share/issues/73)

```javascript
var v='Hello World'; 
alert(v); 
```
弹出“Hello World”

```
var v='Hello World'; 
(function(){ 
alert(v); 
})() 
```

也是弹出了“Hello World”

```javascript
var v='Hello World'; 
(function(){ 
alert(v); 
var v='I love you'; 
})() 
```

弹出了“undefined”
这里面隐藏了一个陷阱，就是JavaScript中的变量提升
它相当于

```javascript
var v='Hello World'; 
(function(){ 
var v;
alert(v); 
v='I love you'; 
})() 
```

变量提升，简单的理解，就是把变量提升提到函数的最顶的地方。需要说明的是，变量提升只是提升变量的声明，并不会把赋值也提升上来，没赋值的变量初始值是undefined。所以上面就出现了声明为undefined的var，因为赋值在后面声明提升在了前面。

```javascript
function foo() {  
    if (false) {  
        var x = 1;  
    }  
    return;  
    var y = 1;  
}  
function foo() {  
    var x, y;  
    if (false) {  
        x = 1;  
    }  
    return;  
    y = 1;  
}  
```

还有一点注意的是因为JavaScript是函数级作用域，只有函数才会创建新的作用域，而不像其他语言有块级作用域，例如：块，就像if语句，在上面例子中，不管会不会进入 if代码块，函数声明都会提升到当前作用域的顶部，得到执行，在js中并不会创建一个新的作用域（在C语言等语言中却是一个新的作用域，上面这个例子，两个函数实际上是一样的）。
从这里我们应该体会到，当我们在写js code的时候，我么需要把变量放在块级作用域的顶端，不然容易发生一些意想不到的错误。
注意：ES5只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。

还有一种就是函数提升

```javascript
function myTest(){ 
    foo(); 
    function foo(){ 
         alert("Hello World"); 
    } 
} 
myTest(); 
```

弹出“Hello World”
这里函数提升成功

```javascript
function myTest(){ 
    foo(); 
    var foo =function foo(){ 
          alert("我来自 foo"); 
    } 
} 
```
myTest(); 
报错：foo不是函数
函数提升失败

## 关于Javascript的函数声明和函数表达式

Javascript定义函数有两种类型

函数声明
```javascript
// 函数声明
function wscat(type){
    return type==="wscat";
}
```

函数表达式

```javascript
// 函数表达式
var oaoafly = function(type){
    return type==="oaoafly";
}
```

先看下面这个经典问题，在一个程序里面同时用函数声明和函数表达式定义一个名为getName的函数

```javascript
getName()//oaoafly
var getName = function() {
    console.log('wscat')
}
getName()//wscat
function getName() {
    console.log('oaoafly')
}
getName()//wscat
```


上面的代码看起来很类似，感觉也没什么太大差别。但实际上，Javascript函数上的一个“陷阱”就体现在Javascript两种类型的函数定义上。

JavaScript 解释器中存在一种变量声明被提升的机制，也就是说函数声明会被提升到作用域的最前面，即使写代码的时候是写在最后面，也还是会被提升至最前面。
而用函数表达式创建的函数是在运行时进行赋值，且要等到表达式赋值完成后才能调用.
 ```javascript
var getName//变量被提升，此时为undefined

getName()//oaoafly 函数被提升 这里受函数声明的影响，虽然函数声明在最后可以被提升到最前面了

var getName = function() {
    console.log('wscat')
}//函数表达式此时才开始覆盖函数声明的定义

getName()//wscat

function getName() {
    console.log('oaoafly')
}
getName()//wscat 这里就执行了函数表达式的值
```

所以可以分解为这两个简单的问题来看清楚区别的本质

```javascript
var getName;
console.log(getName)//undefined
getName()//Uncaught TypeError: getName is not a function
var getName = function() {
    console.log('wscat')
}
var getName;
console.log(getName)//function getName() {console.log('oaoafly')}
getName()//oaoafly
function getName() {
    console.log('oaoafly')
}
```

## 总结：

Javascript中函数声明和函数表达式是存在区别的，函数声明在JS解析时进行函数提升，因此在同一个作用域内，不管函数声明在哪里定义，该函数都可以进行调用。而函数表达式的值是在JS运行时确定，并且在表达式赋值完成后，该函数才能调用。这个微小的区别，可能会导致JS代码出现意想不到的bug，让你陷入莫名的陷阱中。