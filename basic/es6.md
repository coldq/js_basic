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