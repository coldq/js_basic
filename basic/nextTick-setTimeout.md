## 理解setTimeout、setImmediate、process.nextTick的区别

1. `setTimeout` 注册的回调会在事件循环的 `timers`、`poll` 和` closing callbacks `阶段执行。需要注意的是，计时器默认定义的 TIMEOUT_MAX 的取值范围是 `[1, 2 ^ 31 - 1]`，不足 1 或者超过上限都会初始化为 1，也就是说你调用 `setTimeout(fn, 0) `和 `setTimeout(fn, 1) `的效果是一样的。另一点，当timer到延时处理方法到达触发条件，于是将延时处理方法加入**任务队列**，必须等到当前代码（执行栈）执行完，主线程才会去执行它指定的回调函数。没有办法保证，回调函数一定会在setTimeout()指定的时间执行。（setInterval会不管回调，准时调用）

2. `process.nextTick` 方法可以在当前"执行栈"的尾部-->下一次Event Loop（主线程读取"任务队列"）之前-->触发。process指定的回调函数注册的回调会在事件循环的当前阶段结束前执行，而不是只有 `poll`、`check` 阶段才会执行。`process` 是内核模块，运行时是全局上下文，所以 `microtask` 只有一个，无论你是在哪个阶段、哪个闭包内用 `nextTick` 注册的回调都会被 `push` 到`nextTickQueue`，并在事件循环当前阶段结束前执行。

3. `setImmediate` 注册的回调会在 `check` 阶段执行,属于check观察者，其设置的回调函数，会插入到**下次事件循环的末尾**。。因为它需要由 `check watcher `来执行，`check watcher `只在` check `阶段处于 `active` 状态。与 `process.nextTick `不同，setImmediate 因运行时的上下文不同而产生不同的 `ImmediateList`，所以 `microtask `可以有多个。`setImmediate` 会在异常的时候执行` process.nextTick(processImmediate)`，会在当前阶段结束前重新执行一次这个异常任务（即 check 阶段）。Else,创建异步操作的话毋庸置疑是使用setImmediate。


### 举例

```
A();
B();
C();
```

![](/image/1-12-1.jpg)

```
A();
process.nextTick(B);
C();
```

![](/image/1-12-2.jpg)

```
A();
setImmediate(B);
C();
```

![](/image/1-12-3.jpg)


### 参考

[转向Javascript系列-从setTimeout说事件循环模型 I](http://www.alloyteam.com/2015/10/turning-to-javascript-series-from-settimeout-said-the-event-loop-model/)
[深入理解 setTimeout、setImmediate、process.nextTick](https://cnodejs.org/topic/58d7d2f26f8b9bf02d1d0b1b)
[JavaScript 运行机制详解：再谈Event Loop](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)