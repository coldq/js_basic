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

