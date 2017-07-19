## PureRenderMixin 与 PureComponent

为了提高React组件渲染性能，React 针对组件的 shouldComponentUpdate 方法进行了封装处理，我们不需要在每个组件里面手动编写 shouldComponentUpdate。

### PureRenderMixin

React在之前版本提供了 PureRenderMixin 的mixin形式，其用法如下：

```javascript
// react官方demo
import PureRenderMixin from 'react-addons-pure-render-mixin';
class FooComponent extends React.Component {
  constructor(props) {
    super(props);
    this.shouldComponentUpdate = PureRenderMixin.shouldComponentUpdate.bind(this);
  }
```

其原理就是重写了 shouldComponentUpdate 方法。

### PureComponent

React 15.3.0 新增了一个 PureComponent 类，以 ES2015 class 的方式方便地定义纯组件 (pure component)，用于取代之前的 PureRenderMixin。

这个类的用法很简单，如果你有些组件是纯组件，那么把继承类从 Component 换成 PureComponent 即可。当组件更新时，如果组件的 props 和 state 都没发生改变，render 方法就不会触发，省去 Virtual DOM 的生成和比对过程，达到提升性能的目的。



#### 原理

当组件更新时，如果组件的 props 和 state 都没发生改变， render 方法就不会触发，省去 Virtual DOM 的生成和比对过程，达到提升性能的目的。具体就是 React 自动帮我们做了一层浅比较：

```javascript
if (this._compositeType === CompositeTypes.PureClass) {
  shouldUpdate = !shallowEqual(prevProps, nextProps)
  || !shallowEqual(inst.state, nextState);
}
```

而 shallowEqual 又做了什么呢？会比较 Object.keys(state | props) 的长度是否一致，每一个 key 是否两者都有，并且是否是一个引用，也就是只比较了第一层的值，确实很浅，所以深层的嵌套数据是对比不出来的。


这里要注意的是：PureRenderMixin、PureComponent 内进行的仅仅是浅比较对象(shallowCompare)。如果对象包含了复杂的数据结构，深层次的差异可能会产生误判。比如，如果我们的state变为：

```javascript
state = {
     value: { foo: 'bar' }
}

// 每次更改value值的时候进行：
this.setState({ value: newValue });
```

此时直接通过值的比较是行不通的，因为对象的引用关系，导致在子组件里面接受到的 this.props.value 与 nextProps.value 永远都是相等的。

Eg2：

```javascript
class App extends PureComponent {
  state = {
    items: [1, 2, 3]
  }
  handleClick = () => {
    const { items } = this.state;
    items.pop();
    this.setState({ items });
  }
  render() {
    return (<div>
      <ul>
        {this.state.items.map(i => <li key={i}>{i}</li>)}
      </ul>
      <button onClick={this.handleClick}>delete</button>
    </div>)
  }
}
```

会发现，无论怎么点 delete 按钮， li 都不会变少，因为 items 用的是一个引用， shallowEqual 的结果为 true 。改正：

```javascript
handleClick = () => {
  const { items } = this.state;
  items.pop();
  this.setState({ items: [].concat(items) });
}
```

这样每次改变都会产生一个新的数组，也就可以 render 了。这里有一个矛盾的地方，如果没有 items.pop(); 操作，每次 items 数据并没有变，但还是 render 了，数据都不变，你 setState 干嘛？

这里的解决方案主要有：

- 深比较： 原理与深拷贝类似，比较耗时，不推荐

- immutable.js：FaceBook官方提出的不可变数据解决方案，主要解决了复杂数据在deepClone和对比过程中性能损耗

#### 使用

```javascript
import React, { PureComponent } from 'react'

class Example extends PureComponent {
  render() {
    // ...
  }
}
```

#### 兼容旧版本

```javascript  
import React { PureComponent, Component } from 'react';
class Foo extends (PureComponent || Component) {
  //...
}
```

#### 与 shouldComponentUpdate 共存

如果 PureComponent 里有 shouldComponentUpdate 函数的话，直接使用 shouldComponentUpdate 的结果作为是否更新的依据，没有 shouldComponentUpdate 函数的话，才会去判断是不是 PureComponent ，是的话再去做 shallowEqual 浅比较。

```javascript 
// 这个变量用来控制组件是否需要更新
var shouldUpdate = true;
// inst 是组件实例
if (inst.shouldComponentUpdate) {
  shouldUpdate = inst.shouldComponentUpdate(nextProps, nextState, nextContext);
} else {
  if (this._compositeType === CompositeType.PureClass) {
    shouldUpdate = !shallowEqual(prevProps, nextProps) ||
      !shallowEqual(inst.state, nextState);
  }
}
```

### 参考

[React PureComponent 使用指南](https://wulv.site/2017-05-31/react-purecomponent.html)

