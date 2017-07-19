## angularJs和react比较

### ReactJS快速回顾

ReactJS是一套JavaScript Web库，由Facebook打造而成且主要用于构建高性能及响应式用户界面。React负责解决其它javascript框架所面对的一大常见难题，即对大规模数据集的处理。能够使用虚拟DOM并在发生变更时利用补丁安装机制只对DOM中的dirty部分进行重新渲染，React得以实现远超其它框架的速度表现。

优势:

1. React伟大之处就在于，提出了Virtual Dom这种新颖的思路，并且这种思路衍生出了React Native，有可能会统一Web/Native开发。在性能方面，由于运用了Virtual Dom技术，Reactjs只在调用setState的时候会更新dom，而且还是先更新Virtual Dom，然后和实际Dom比较，最后再更新实际Dom。这个过程比起AngularJS的bind方式来说，一是更新dom的次数少，二是更新dom的内容少，速度肯定快。

2. ReactJS更关注UI的组件化，和数据的单向更新，提出了FLUX架构的新概念，现在React可以直接用js ES6语法了，然后通过webpack编译成浏览器兼容的ES5，开发效率上有些优势. 
react native生成的App不是运行在WebView上，而是系统原生的UI，React通过jsx生成系统原生的UI，iOS和Android的React UI组件还是比较相似的，大量代码可以复用

3. 维护UI的状态,Angular 里面使用的是 $scope，在 React 里面使用的是 this.setState。 而 React 的好处在于，它更简单直观。所有的状态改变都只有唯一一个入口 this.setState()，Angular 就比较复杂，不知道背后使用了哪些黑魔法。

劣势:

React是目标是UI组件，通常可以和其它框架组合使用，目前并不适合单独做一个完整的框架。React 即使配上 Flux 的组合，也不能称之一个完整的框架，比如你想用Promise化的AJAX？对不起没有，自己找现成的库去。而且第三方组件远远不如Angular多。目前在大的稳定的项目上采用React的，我也就只知道有Yahoo的Email。React本身只是一个V而已，所以如果是大型项目想要一套完整的框架的话，也许还需要引入Flux和route相关的东西。而Angular在这方面提供的东西比React多多了。


### Angular 2满载强化机制

Angular 2相较于Angular 1迎来一系列强化。首先，Angular 2高度关注创建可复用的前端组件。尽管Angular 1在一定程度上也能实现同样的效果，但该框架的新版本解决了大量影响利用性的难题，例如对$scope与控制器的依赖性。其指令亦得到显著简化，使得Angular 2代码较前代更易于输入及阅读。Angular 2还考虑到与TypeScript的协作需求，消除了大量用于保证类型安全的代码。再加上众多性能与框架改进，Angular 2确实给人焕然一新之感。

优势:

1. angularjs是一套完整的框架，angular有自带的数据绑定、render渲染、angularUI库,过滤器,filter,directive(模板),service(服务),q(defer),route,http，cookie,inject(依赖注入),factory,provider……，等等一系列工具，基本上只要你在做web开发用过的东西，它都有一个。但是这些东西react自身都没有。

2. Angularjs的架构清晰，分工明确，扩展性良好，model，view，controller谁在什么时候做什么事情说的很清楚，angular能够让程序员真正专注于业务逻辑，对js能力要求也不高（基本上只需要写业务逻辑，实在用不上那些高级的js技巧和知识呀），而且因为对html侵入不大，非常易于和designer协作。整个框架充满了DI的思路，耦合性非常低，对象都是被inject的，也就是说每个对象都可以轻易被替换而不影响其他对象。

劣势:

1. 性能 
同样是TODO MVC的Sample，Angular完全载入用了1.1s（WebPagetest - Visual Comparison）。React只用了0.3s。

2. Angular 2.0推翻重做使得目前不宜采用此框架
Angular 1.x版本其实是个比较旧的东西了，现在看来有些理念过时了，比如依赖注入、自己独特的模块化，这些东西其实在ES6下已经很好地被解决了。

### 注意事项

值得强调的是，React与Angular（任意版本）之间的比较其实并不对等。

Angular是一套前端框架，负责为应用客户端提供完整架构，并允许我们将客户端代码作为强大的功能套件。

而React则偏向于一套工具库，其提供的功能并不丰富——其主要作用是充当完整项目的组成部分，而非主导整体代码结构。