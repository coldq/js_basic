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

```javascript
import React, { PureComponent } from 'react'

class Example extends PureComponent {
  render() {
    // ...
  }
}
```

这里要注意的是：PureRenderMixin、PureComponent 内进行的仅仅是浅比较对象(shallowCompare)。如果对象包含了复杂的数据结构，深层次的差异可能会产生误判。比如，如果我们的state变为：

```javascript
state = {
     value: { foo: 'bar' }
}

// 每次更改value值的时候进行：
this.setState({ value: newValue });
```

此时直接通过值的比较是行不通的，因为对象的引用关系，导致在子组件里面接受到的 this.props.value 与 nextProps.value 永远都是相等的。这里的解决方案主要有：

- 深比较： 原理与深拷贝类似，比较耗时，不推荐

- immutable.js：FaceBook官方提出的不可变数据解决方案，主要解决了复杂数据在deepClone和对比过程中性能损耗