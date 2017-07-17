## ES6相关知识记录

持续更新... :imp:

#### 展开运算符
展开运算符（spread）是三个点（…）,可以将数组转为用逗号分隔的参数序列。在数组合并、函数传参，会快捷很多。

```
let arr = [2,3,4]
console.log([1,...arr,5]);//[1,2,3,4,5]

//函数传参
function myFunction(x, y, z) { }
let args = [0, 1, 2];
myFunction(...args);

//数据解构
let cold = ['autumn', 'winter'];
let warm = ['spring', 'summer'];
// 析构数组
let otherSeasons, autumn;
[autumn, ...otherSeasons] = cold;
otherSeasons      // => ['winter']
```

#### 模板字符串

ES6引入了一种新型的字符串字面量语法，我们称之为模板字符串（template strings）。除了使用反撇号字符 ` 代替普通字符串的引号 ' 或 " 外，它们看起来与普通字符串并无二致。在最简单的情况下，它们与普通字符串的表现一致：

```
let str = `Ceci n'est pas une chaîne.`;
typeof str //"string"
```

它为JavaScript提供了简单的字符串插值功能，从此以后，你可以通过一种更加美观、更加方便的方式向字符串中插值了。

```
let name = 'Tom';
console.log(`my name is ${name}`);//my name is Tom
```

模板字符串的一些细节：

- 模板占位符中的代码可以是任意JavaScript表达式，所以函数调用、算数运算等这些都可以作为占位符使用，你甚至可以在一个模板字符串中嵌套另一个，我称之为模板套构（template inception）。

- 如果这两个值都不是字符串，可以按照常规将其转换为字符串。例如：如果action是一个对象，将会调用它的.toString()方法将其转换为字符串值。

- 如果你需要在模板字符串中书写反撇号，你必须使用反斜杠将其转义：\`\\\`\` 等价于 "\`"。

- 同样地，如果你需要在模板字符串中引入字符$和{。无论你要实现什么样的目标，你都需要用反斜杠转义每一个字符：\`\\$\`和\`\\{\`。

与普通字符串不同的是，模板字符串可以多行书写：

```
let s = `ww
ee
rr`

//s:
//"ww
//ee
//rr"
```

模板字符串中所有的空格、新行、缩进，都会原样输出在生成的字符串中。

### ES6定义方式

ES5有两种声明变量的方法，var命令 和function命令。

ES6除了以上两种方法外还有：let命令、const命令、import命令、class命令

#### var

ES5中最原始的变量声明，用于声明变量，其实JavaScript是弱类型语言，对数据类型变量要求不太严格，所以不必声明每一个变量的类型（这就是下面说的隐式声明，当然这并不是一个好习惯），在使用变量之前先进行声明是一种好的习惯。

##### 1.作用域

使用var声明的变量的作用域是函数作用域（在ES5时代，只有函数作用域和全局作用域两种作用域），在一个函数内用var声明的变量，则只在这个函数内有效。
```
function test(){
    var a;
    console.log(a);//undefined
}
console.log(a);//ReferenceError: a is not defined
```

##### 2.变量声明提升

用var声明变量时，只要在一个函数作用域内，无论在什么地方声明变量，都会把变量的声明提升到函数作用域的最前头，所以无论使用变量在变量声明前还是声明后，都不会报错（当然只是声明提前，赋值并没有提前，所以如果使用在声明之前，会输出undefined，但不会报错）。
```
function test(){
    console.log(a);//undefined
    var a=3;
}
```

##### 隐式声明

当没有声明，直接给变量赋值时，会隐式地给变量声明，此时这个变量作为全局变量存在。
```
function test(){
    a=3;
    console.log(a);//3
}
test();
console.log(a);//3
```
当然要注意，隐式声明的话就没有变量声明提前的功能了，所以下面的使用是会报错的。
```
function test(){
    console.log(a);//ReferenceError: a is not defined
    a=3;
}
```

#### function

用function声明的是函数对象，作用域与var一样，是函数作用域。

```
function test(){
    function a(){
        console.log('d');
    }
    a();//'d'
}
a();//ReferenceError: a is not defined
```

同样，function声明也有变量声明提升，下面是两个特殊的例子：

```
function hello1(a){
       console.log(a); //[Function: a]
    function a(){}
    console.log(a);//[Function: a]
}
hello1('test');   

function hello2(a){
       console.log(a); //test
    var a=3；
    console.log(a);//3
}
hello2('test');
```

这里有涉及到函数中形参的声明，我们可以将以上两个例子看成：

```
function hello1(a){
    var a='test；
       console.log(a); //[Function: a]
    function a(){}
    console.log(a);//[Function: a]
}
hello1('test');   

function hello2(a){
    var a='test；
       console.log(a); //test
    var a=3；
    console.log(a);//3
}
hello2('test');
```

可以看到函数对象的声明也提前了，但是在形参变量声明之后（形参的变量声明在所有声明之前）。

当函数对象和普通对象同时声明时，函数对象的声明提前在普通对象之后。

```
function test(){     
    console.log(a);//[Function: a]
    function a(){}
    var a;
    console.log(a);//[Function: a]
}
```
#### let

ES6新增的声明变量的关键字，与var类似。

当然，与var也有很大区别：

##### 1.作用域不同

let声明的变量的作用域是块级作用域（之前的js并没有块级作用域，只有函数作用域和全局作用域），var声明的变量的作用域是函数作用域。
```
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```
2.不存在变量声明提升

用var声明变量时，只要在一个函数作用域内，无论在什么地方声明变量，都会把变量的声明提升到函数作用域的最前头，所以无论使用变量在变量声明前还是声明后，都不会报错。而let不一样，与java以及其他语言一样，let声明的变量，在未声明之前变量是不存在的。（js的语法越来越向java靠拢）

```
console.log(a); // undefined,但是不报错。
console.log(b); // ReferenceError: b is not defined.

var a = 2;
let b = 2;
```

注意：在使用babel时可能会遇到这样的情况：

```
console.log(b); //undefined
let b = 2;
```

babel在翻译es6时，似乎直接将let变为了var，所以运行时也有变量声明提升了，但是在Chrome下运行时是正确的
let

ES6新增的声明变量的关键字，与var类似。

当然，与var也有很大区别：

1.作用域不同

let声明的变量的作用域是块级作用域（之前的js并没有块级作用域，只有函数作用域和全局作用域），var声明的变量的作用域是函数作用域。

{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
2.不存在变量声明提升

用var声明变量时，只要在一个函数作用域内，无论在什么地方声明变量，都会把变量的声明提升到函数作用域的最前头，所以无论使用变量在变量声明前还是声明后，都不会报错。而let不一样，与java以及其他语言一样，let声明的变量，在未声明之前变量是不存在的。（js的语法越来越向java靠拢）

console.log(a); // undefined,但是不报错。
console.log(b); // ReferenceError: b is not defined.

var a = 2;
let b = 2;
注意：在使用babel时可能会遇到这样的情况：

console.log(b); //undefined
let b = 2;
babel在翻译es6时，似乎直接将let变为了var，所以运行时也有变量声明提升了，但是在Chrome下运行时是正确的。

##### 3.暂时性死区

所谓暂时性死区，意思是，在一个块级作用域中，变量唯一存在，一旦在块级作用域中用let声明了一个变量，那么这个变量就唯一属于这个块级作用域，不受外部变量的影响，如下面所示。

无论在块中的任何地方声明了一个变量，那么在这个块级作用域中，任何使用这个名字的变量都是指这个变量，无论外部是否有其他同名的全局变量。

暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。

暂时性死区的意义也是让我们标准化代码，将所有变量的声明放在作用域的最开始。
```
var a = 123;  
{
 console.log(a);//ReferenceError
  let a;
}
```
##### 4.不允许重复声明

在相同的作用域内，用let声明变量时，只允许声明一遍。 （var是可以多次声明的）
```
// 正确
function () {
  var a = 10;
  var a = 1;
}

// 报错，Duplicate declaration "a"
function () {
  let a = 10;
  var a = 1;
}

// 报错,Duplicate declaration "a"
function () {
  let a = 10;
  let a = 1;
}
```


