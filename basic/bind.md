## bind

bind()方法创建一个新的函数, 当被调用时，将其this关键字设置为提供的值，在调用新函数时，在任何提供之前提供一个给定的参数序列。

bind和call、apply的使用区别：
- 都是用来改变函数的this对象的指向的；
- 第一个参数都是this要指向的对象；
- 都可以利用后续参数传参；
- bind是返回对应函数，便于稍后调用；apply、call是立即调用。

### 语法
  fun.bind(thisArg[, arg1[, arg2[, ...]]])
#### 参数

##### thisArg
当绑定函数被调用时，该参数会作为原函数运行时的 this 指向。当使用new 操作符调用绑定函数时，该参数无效。

##### arg1, arg2, ...
当绑定函数被调用时，这些参数将置于实参之前传递给被绑定的方法。

##### 返回值
返回由指定的this值和初始化参数改造的原函数拷贝

### 描述

bind() 函数会创建一个新函数（称为绑定函数），新函数与被调函数（绑定函数的目标函数）具有相同的函数体（在 ECMAScript 5 规范中内置的call属性）。当目标函数被调用时 this 值绑定到 bind() 的第一个参数，该参数不能被重写。绑定函数被调用时，bind() 也接受预设的参数提供给原函数。一个绑定函数也能使用new操作符创建对象：这种行为就像把原函数当成构造器。提供的 this 值被忽略，同时调用时的参数被提供给模拟函数。
 
### 示例
#### 创建绑定函数

bind() 最简单的用法是创建一个函数，使这个函数不论怎么调用都有同样的 this 值。JavaScript新手经常犯的一个错误是将一个方法从对象中拿出来，然后再调用，希望方法中的 this 是原来的对象。（比如在回调中传入这个方法。）如果不做特殊处理的话，一般会丢失原来的对象。从原来的函数和原来的对象创建一个绑定函数，则能很漂亮地解决这个问题：

```
this.x = 9; 
var module = {
  x: 81,
  getX: function() { return this.x; }
};

module.getX(); // 返回 81

var retrieveX = module.getX;
retrieveX(); // 返回 9, 在这种情况下，"this"指向全局作用域

// 创建一个新函数，将"this"绑定到module对象
// 新手可能会被全局的x变量和module里的属性x所迷惑
var boundGetX = retrieveX.bind(module);
boundGetX(); // 返回 81
```

#### 偏函数（Partial Functions）

bind()的另一个最简单的用法是使一个函数拥有预设的初始参数。这些参数（如果有的话）作为bind()的第二个参数跟在this（或其他对象）后面，之后它们会被插入到目标函数的参数列表的开始位置，传递给绑定函数的参数会跟在它们的后面。

```
function list() {
  return Array.prototype.slice.call(arguments);
}

var list1 = list(1, 2, 3); // [1, 2, 3]

// Create a function with a preset leading argument
var leadingThirtysevenList = list.bind(undefined, 37);

var list2 = leadingThirtysevenList(); // [37]
var list3 = leadingThirtysevenList(1, 2, 3); // [37, 1, 2, 3]
```

#### 配合 setTimeout

在默认情况下，使用 window.setTimeout() 时，this 关键字会指向 window （或全局）对象。当使用类的方法时，需要 this 引用类的实例，你可能需要显式地把 this 绑定到回调函数以便继续使用实例。

```
function LateBloomer() {
  this.petalCount = Math.ceil(Math.random() * 12) + 1;
}

// Declare bloom after a delay of 1 second
LateBloomer.prototype.bloom = function() {
  window.setTimeout(this.declare.bind(this), 1000);
};

LateBloomer.prototype.declare = function() {
  console.log('I am a beautiful flower with ' +
    this.petalCount + ' petals!');
};

var flower = new LateBloomer();
flower.bloom();  // 一秒钟后, 调用'declare'方法
```

#### 作为构造函数使用的绑定函数

**警告 :这部分演示了 JavaScript 的能力并且记录了 bind() 的超前用法。以下展示的方法并不是最佳的解决方案且可能不应该用在任何生产环境中。**

自然而然地，绑定函数适用于用new操作符 new 去构造一个由目标函数创建的新的实例。当一个绑定函数是用来构建一个值的，原来提供的 this 就会被忽略。然而, 原先提供的那些参数仍然会被前置到构造函数调用的前面。

```
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function() { 
  return this.x + ',' + this.y; 
};

var p = new Point(1, 2);
p.toString(); // '1,2'

var emptyObj = {};
var YAxisPoint = Point.bind(emptyObj, 0/*x*/);
// 以下这行代码在 polyfill 不支持,
// 在原生的bind方法运行没问题:
//(译注：polyfill的bind方法如果加上把bind的第一个参数，即新绑定的this执行Object()来包装为对象，Object(null)则是{}，那么也可以支持)
var YAxisPoint = Point.bind(null, 0/*x*/);

var axisPoint = new YAxisPoint(5);
axisPoint.toString(); // '0,5'

axisPoint instanceof Point; // true
axisPoint instanceof YAxisPoint; // true
new Point(17, 42) instanceof YAxisPoint; // true
```

你知道不需要做特别的处理就可以用new操作符 new 创建一个绑定函数。必然地，你需要知道不需要做特别处理就可以创建一个可以被直接调用的绑定函数，即使你更希望绑定函数是用new操作符 new 来调用。

```
// 这个例子可以直接在你的 javascript 控制台运行
// ...接着上面的代码继续(译注：

// 仍然能作为一个普通函数来调用
// (即使通常来说这个不是被期望发生的)
YAxisPoint(13);

emptyObj.x + ',' + emptyObj.y;   //  '0,13'
```

如果你希望一个绑定函数只支持使用new操作符 new，或者只能直接调用它，那么模板函数必须强制执行那限制。

#### 快捷调用

在你想要为一个需要特定的 this 值的函数创建一个捷径（shortcut）的时候，bind() 方法也很好用。

你可以用 Array.prototype.slice 来将一个类似于数组的对象（array-like object）转换成一个真正的数组，就拿它来举例子吧。你可以创建这样一个捷径：

```
var slice = Array.prototype.slice;

// ...

slice.apply(arguments);
```

用 bind() 可以使这个过程变得简单。在下面这段代码里面，slice 是 Function.prototype 的 call() 方法的绑定函数，并且将 Array.prototype 的 slice() 方法作为 this 的值。这意味着我们压根儿用不着上面那个 apply() 调用了。

```
// same as "slice" in the previous example
var unboundSlice = Array.prototype.slice;
var slice = Function.prototype.call.bind(unboundSlice);

// ...

slice(arguments);
// ==  Function.prototype.call(unboundSlice,arguments) ===>
// [[Call]](unbundSlice,arguments)

var str = Function.prototype.call.bind(Object.prototype.toString);
str([]) //'[object Array]'
```

[再次认识Function.prototype.call](http://www.cnblogs.com/fsjohnhuang/p/4160942.html)


#### Polyfill（兼容旧浏览器）

bind 函数在 ECMA-262 第五版才被加入；它可能无法在所有浏览器上运行。你可以部份地在脚本开头加入以下代码，就能使它运作，让不支持的浏览器也能使用 bind() 功能。

```
if (!Function.prototype.bind) {
  Function.prototype.bind = function (oThis) {
    if (typeof this !== "function") {
      // closest thing possible to the ECMAScript 5
      // internal IsCallable function
      throw new TypeError("Function.prototype.bind - what is trying to be bound is not callable");
    }

    var aArgs = Array.prototype.slice.call(arguments, 1), 
        fToBind = this, 
        fNOP = function () {},
        fBound = function () {
          return fToBind.apply(this instanceof fNOP
                                 ? this
                                 : oThis || this,
                               aArgs.concat(Array.prototype.slice.call(arguments)));
        };

    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP();

    return fBound;
  };
}
```

上述算法和实际的实现算法还有许多其他的不同 （尽管可能还有其他不同之处，却没有那个必要去穷尽）：

- 这部分实现依赖于Array.prototype.slice()， Array.prototype.concat()， Function.prototype.call()这些原生方法。

- 这部分实现创建的函数的实现并没有caller 以及会在 get，set或者deletion上抛出TypeError错误的 arguments 属性这两个不可改变的“毒药” 。（假如环境支持{jsxref("Object.defineProperty")}}， 或者实现支持__defineGetter__ and __defineSetter__ 扩展）

- 这部分实现创建的函数有 prototype 属性。（正确的绑定函数没有的）

- 这部分实现创建的绑定函数所有的 length 属性并不是同ECMA-262标准一致的：它的 length 是0，而在实际的实现中根据目标函数的 length 和预先指定的参数个数可能会返回非零的 length。

- 如果你选择使用这部分实现，你不能依赖于ECMA-262，但是ECMA-5是可以的。在某些情况下（也可以作另一番修改以适应特定的需要），这部分实现也许可以作为一个过渡，在bind()函数被广泛支持之前。