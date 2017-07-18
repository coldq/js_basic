## Promise 原理&实现

### 原理

Javascript 采用回调函数(callback)来处理异步编程。从同步编程到异步回调编程有一个适应的过程，但是如果出现多层回调嵌套，也就是我们常说的厄运的回调金字塔(Pyramid of Doom)，绝对是一种糟糕的编程体验。于是便有了 CommonJS 的 Promises/A 规范，用于解决回调金字塔问题。

#### 什么是 Promise
一个 Promise 对象代表一个目前还不可用，但是在未来的某个时间点可以被解析的值。它允许你以一种同步的方式编写异步代码。例如，如果你想要使用 Promise API 异步调用一个远程的服务器，你需要创建一个代表数据将会在未来由 Web 服务返回的 Promise 对象。唯一的问题是目前数据还不可用。当请求完成并从服务器返回时数据将变为可用数据。在此期间，Promise 对象将扮演一个真实数据的代理角色。接下来，你可以在 Promise 对象上绑定一个回调函数，一旦真实数据变得可用这个回调函数将会被调用。

Promise 对象曾经以多种形式存在于许多语言中。

#### 去除厄运的回调金字塔(Pyramid of Doom)
Javascript 中最常见的反模式做法是回调内部再嵌套回调。

```
// 回调金字塔
asyncOperation(function(data){
  // 处理 `data`
  anotherAsync(function(data2){
      // 处理 `data2`
      yetAnotherAsync(function(){
          // 完成
      });
  });
});
```
引入 Promises 之后的代码

```
promiseSomething()
.then(function(data){
    // 处理 `data`
    return anotherAsync();
})
.then(function(data2){
    // 处理 `data2`
    return yetAnotherAsync();
})
.then(function(){
    // 完成
});
```

#### Promises/A 规范

promise 表示一个最终值，该值由一个操作完成时返回。

- promise 有三种状态：**未完成** (unfulfilled)，**完成** (fulfilled) 和**失败** (failed)。
- promise 的状态只能由**未完成**转换成完成，或者**未完成**转换成**失败** 。
- promise 的状态转换只发生一次。

promise 有一个 then 方法，then 方法可以接受 3 个函数作为参数。前两个函数对应 promise 的两种状态 fulfilled 和 rejected 的回调函数。第三个函数用于处理进度信息（对进度回调的支持是可选的）。

```
promiseSomething().then(function(fulfilled){
        //当promise状态变成fulfilled时，调用此函数
    },function(rejected){
        //当promise状态变成rejected时，调用此函数
    },function(progress){
        //当返回进度信息时，调用此函数
});
```

如果 promise 支持如下连个附加方法，称之为可交互的 promise

- get(propertyName)
获得当前 promise 最终值上的一个属性，返回值是一个新的 promise。

- call(functionName, arg1, arg2, ...)
调用当然 promise 最终值上的一个方法，返回值也是一个新的promise。

### 实现

Promise的实现，主要包括定义：

- 自身构造器，包括定义Promise状态，resolve和reject这两个函数（来进行状态的改变）；

- then方法：用来注册在这个Promise状态确定后的回调，很明显，then方法需要写在原型链上。then方法会返回一个Promise，关于这一点，Promise/A+标准并没有要求返回的这个Promise是一个新的对象，但在Promise/A标准中，明确规定了then要返回一个新的对象，目前的Promise实现中then几乎都是返回一个新的Promise(详情)对象，所以在我们的实现中，也让then返回一个新的Promise对象。

[理解 Promise 的工作原理](https://blog.coding.net/blog/how-do-promises-work)


[史上最易读懂的 Promise 实现](https://zhuanlan.zhihu.com/p/21834559)

[采用Symbol和process.nextTick实现Promise](https://github.com/slashhuang/blog/blob/master/essays/nodejs/Promise.md)