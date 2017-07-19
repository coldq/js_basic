## React diff　算法

React 中最值得称道的部分莫过于 Virtual DOM 与 diff 的完美结合，特别是其高效的 diff 算法，这让我们可以无需担心性能问题而”毫无顾忌”的随时“刷新”整个页面，由虚拟DOM来确保只对界面上真正变化的部分进行实际的DOM操作。

### 什么是DOM Diff算法

Web界面由DOM树来构成，当其中某一部分发生变化时，其实就是对应的某个DOM节点发生了变化。在React中，构建UI界面的思路是由当前状态决定界面。前后两个状态就对应两套界面，然后由React来比较两个界面的区别，这就需要对DOM树进行Diff算法分析。

即给定任意两棵树，找到最少的转换步骤。但是标准的的Diff算法复杂度需要O(n^3)，这显然无法满足性能要求。要达到每次界面都可以整体刷新界面的目的，势必需要对算法进行优化。这看上去非常有难度，然而Facebook工程师却做到了，他们结合Web界面的特点做出了两个简单的假设，使得Diff算法复杂度直接降低到O(n)

1. 两个相同组件产生类似的DOM结构，不同的组件产生不同的DOM结构；
2. 对于同一层次的一组子节点，它们可以通过唯一的id进行区分。

### Dom节点树 diff

React的整个内容其实就是一棵Virtual Node组成的Tree。状态的改变会导致Tree变化。其Diff算法按照简单而准确的规则，用较高的性能进行了dom重绘。

#### 按照层级

找到两棵任意的树之间最小的修改是一个复杂度为 O(n^3) 的问题.
React 用了一种简单但是强大的技巧, 达到了接近 O(n) 的复杂度.

React 仅仅是尝试把树按照层级分解. 这彻底简化了复杂度,而且也不会失去很多, 因为 Web 应用很少有 component 移动到树的另一个层级去。它们大部分只是在相邻的子节点之间移动。

![](/image/5-1-1.png)

React只会对相同颜色方框内的DOM节点进行比较，即同一个父节点下的所有子节点。

当发现节点已经不存在，则该节点及其子节点会被完全删除掉，不会用于进一步的比较。这样只需要对树进行一次遍历，便能完成整个DOM树的比较。

![](/image/5-1-2.png)

#### Component

React 是基于组件构建应用的，一个组件可以看做virtual DOM tree中的一棵子树。

- 如果是同一类型（相同 class）的组件，按照原策略继续比较 virtual DOM tree。

- 如果不是，则将该组件判断为 dirty component，从而替换整个组件下的所有子节点。

![](/image/5-1-3.png)

如图，当 component D 改变为 component G 时，即使这两个 component 结构相似，一旦 React 判断 D 和 G 是不同类型的组件，就不会比较二者的结构，而是直接删除 component D，重新创建 component G 以及其子节点。虽然当两个 component 是不同类型但结构相似时，React diff 会影响性能，但正如 React 官方博客所言：不同类型的 component 是很少存在相似 DOM tree 的机会，因此这种极端因素很难在实现开发过程中造成重大影响的。

#### 节点的比较

在React中比较两个虚拟DOM节点,分为两种情况：

1. 节点类型不同 (类似组件比较）
当在树中的同一位置前后输出了不同类型的节点，React直接删除前面的节点，然后创建并插入新的节点。假设我们在树的同一位置前后两次输出不同类型的节点。

2. 节点类型相同，但是属性不同。

当在树中的同一位置前后输出了不同类型的节点，React直接删除前面的节点，然后创建并插入新的节点。假设我们在树的同一位置前后两次输出不同类型的节点。
```javascript
var MyComponent = React.createClass({
  render: function() {
    if (this.props.first) {
      return <div className="first"><span>A Span</span></div>;
    } else {
      return <div className="second"><p>A Paragraph</p></div>;
    }
  }
});
```
