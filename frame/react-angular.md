## angularJs和react比较

ReactJS快速回顾

ReactJS是一套JavaScript Web库，由Facebook打造而成且主要用于构建高性能及响应式用户界面。React负责解决其它javascript框架所面对的一大常见难题，即对大规模数据集的处理。能够使用虚拟DOM并在发生变更时利用补丁安装机制只对DOM中的dirty部分进行重新渲染，React得以实现远超其它框架的速度表现。

优势:

1.React伟大之处就在于，提出了Virtual Dom这种新颖的思路，并且这种思路衍生出了React Native，有可能会统一Web/Native开发。在性能方面，由于运用了Virtual Dom技术，Reactjs只在调用setState的时候会更新dom，而且还是先更新Virtual Dom，然后和实际Dom比较，最后再更新实际Dom。这个过程比起AngularJS的bind方式来说，一是更新dom的次数少，二是更新dom的内容少，速度肯定快。

2.ReactJS更关注UI的组件化，和数据的单向更新，提出了FLUX架构的新概念，现在React可以直接用js ES6语法了，然后通过webpack编译成浏览器兼容的ES5，开发效率上有些优势. 
react native生成的App不是运行在WebView上，而是系统原生的UI，React通过jsx生成系统原生的UI，iOS和Android的React UI组件还是比较相似的，大量代码可以复用

3.维护UI的状态,Angular 里面使用的是 $scope，在 React 里面使用的是 this.setState。 而 React 的好处在于，它更简单直观。所有的状态改变都只有唯一一个入口 this.setState()，Angular 就比较复杂，不知道背后使用了哪些黑魔法。

劣势:

React是目标是UI组件，通常可以和其它框架组合使用，目前并不适合单独做一个完整的框架。React 即使配上 Flux 的组合，也不能称之一个完整的框架，比如你想用Promise化的AJAX？对不起没有，自己找现成的库去。而且第三方组件远远不如Angular多。目前在大的稳定的项目上采用React的，我也就只知道有Yahoo的Email。React本身只是一个V而已，所以如果是大型项目想要一套完整的框架的话，也许还需要引入Flux和route相关的东西。而Angular在这方面提供的东西比React多多了.