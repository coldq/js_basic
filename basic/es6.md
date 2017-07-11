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

模板字符串中所有的空格、新行、缩进，都会原样输出在生成的字符串中
