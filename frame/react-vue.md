## Vue和React的使用场景和深度有何不同？

在超大量数据的首屏渲染速度上，React 有一定优势，因为 Vue 的渲染机制启动时候要做的工作比较多，而且 React 支持服务端渲染。

React 配合严格的 Flux 架构，适合超大规模多人协作的复杂项目。

理论上 Vue 配合类似架构也可以胜任这样的用例，但缺少类似 Flux 这样的官方架构。

小快灵的项目上，Vue 和 React 的选择更多是开发风格的偏好。对于需要对 DOM 进行很多自定义操作的项目，Vue 的灵活性优于 React。

开发风格的偏好：React 推荐的做法是 JSX + inline style，也就是把 HTML 和 CSS 全都整进 JavaScript 了。Vue 的默认 API 是以简单易上手为目标，但是进阶之后推荐的是使用 webpack + vue-loader 的单文件组件格式：

作者：尤雨溪
链接：https://www.zhihu.com/question/31585377/answer/52576501
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


