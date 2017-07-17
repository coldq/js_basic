## 响应式设计：自定义网格布局

一个网格可以理解为水平与垂直方向相互相交的二维结构体，是用于展示结构性内容。网格是作为框架，为开发者有效地组织文本和图片提供简单的接口。一个完整、适当的网格系统增强了扩展性和维护，也包括手机应用的可读性。

网格系统的好处多不胜数；基于网格结构...

- 为展示内容提供顺序性、创意性和和谐性；
- 允许用户可预知哪里有自己需要的信息；
- 很方便地添加新的内容，而不怕脱节或者被边缘化。

为了构建一个完整的网格布局，第一步是要去创建本身的画布结构。画布的大小将决定网格框架大小：画布将被平均分割成3、5、9、12等分，这是界面或者布局的开始工作。

网格尺寸不必要被定义为一个固定值。一个很好的实践方法就是让内容来决定网格布局的尺寸，反之也是成立的。那么，“内容”并不是专指文本内容；而是指界面上所有的东西，包括图像、视频、文字、广告、链接等等。而不是尝试把界面所有东西塞到固定的网格里，你应该根据内容来设计适合的网格尺寸。在你定制网格结构时,布局被垂直或水平划分线划分,从而提供一致的组织空间。

### 在线工具

目前来说，基于HTML和CSS的web设计框架已非常著名，很多强大的框架也是基于网格布的。互联网上有很多基于CSS有用的模型和框架，可帮助设计师来创建一个简单、快速、有效的网格布局。有些布局是很灵活的，其他的是限制相当严格的（原因是为了限制内容）。有些网格布局只有3到5列（最大值），而有些却有12到16列。这里举例子想说明网格布局不是一直都是相同的，不是某种布局就一定比其他的好。然而，为了全面地了解基于网格布局方案的好处，应该为每个项目考虑一个独特的模式。你不要害怕尝试多种方案；需要记住的是，最后获胜的，往往是简单的设计。

现在网络上，有许多有用的网格资源可为你的页面或项目创建一个合适的布局。

- [Grid System 960](http://960.gs/):此工具允许你使用一个960像素网格尺寸来创建网站。选择960，是因为很容易分成不同的列和行。

- [Gridr Buildrrr](http://gridr.atomeye.com/):此工具为自定义项目提供精确地控制边界、外边距和盒子内容。

- [Design by Grid](http://www.designbygrid.com/):创建基于网格的杂志内容。

### 网格布局结构

如果你想尝试自己开发一个网格结构，确定一个界面宽度和在不同的列之间的间距。请记住，为了获得稳定效果，不同的水平间距大小都是相等的，这点是很重要的。正如上面所说的，先去研究内容，思考如何定位和安排，并保持这些想法贯穿在整个实际开发过程中。

把你的app或者移动网站的主要元素（header，content，sidebar，和footer）添加到一个容器“wrapper”里，代码如下：

```
<div id="wrapper">
    <div id="header">Header</div>
    <div id="content">Content</div>
    <div id="sidebar">Sidebar</div>
    <div id="footer">Footer</div>
</div>
```

这里你的内容“content”块和侧栏“sidebar”块还会分层多个“section”块，“section”块可以命名为Grid1（网格1）、Grid2（网格2）、Grid3（网格3）等等（同时，这取决于你创建多少个网格）。在每一个网格单元内，还可以添加很多你想要的子section。

```
<div class="griglia1">
    <div>Subsection 1</div>
    <div>Subsection 2</div>
    <div>Subsection 3</div>
</div>
```

至于CSS，几乎所有元素都是基于CSS的float（浮动）属性的应用。为了你的网格设计突出的区域块能够正确定位，你会使用所谓的“相对浮动”技术。为了水平间隙包含着网格之间，首先你需要去创建多个自包含的列，然后，你需要为每一个设置一个浮动和固定宽度。我的实现代码如下：

```
div.grid1{
  float: left;
  width: 290px;
}
```

接着，依然使用于包含在容器内的所有子Section，指定宽度大小和右外边距（例子是10像素）来分隔开来，如下：

```
div.grid2 div{
  float: left;
  width: 250px;
  margin-right: 10px;
}
```

最后，为了实现模块化，使得Section占的空间在两个或以上的列，我们使用所谓的“联合列”:你只需要为一个元素指定一个类（class），例如ext2、ext3,ext4或ext5，分别代表占2、3、4或5列，而不是确定宽度值。

### 总结

本文尝试去指明一些响应式布局或者界面开发与设计可能使用的技术。对于网格，如果说要记住一个原则，那就是:不要给你的内容塞不合身的网格。如果流行的解决方案不满足你的需求，可以设计定制网格完美地适配你的内容。可快速、轻松地使用几列、行以及CSS声明即可解决。

著作权归作者所有。
商业转载请联系作者获得授权,非商业转载请注明出处。
原文: http://www.w3cplus.com/responsive/responsive-web-design-grid-layouts.html