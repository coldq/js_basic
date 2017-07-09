## display:inline-block 和 float 水平排列区别

1、文档流（Document flow）:浮动元素会脱离文档流，并使得周围元素环绕这个元素。而inline-block元素仍在文档流内。因此设置inline-block不需要清除浮动。当然，周围元素不会环绕这个元素，你也不可能通过清除inline-block就让一个元素跑到下面去。

2、水平位置（Horizontal position）：很明显你不能通过给父元素设置text- align:center让浮动元素居中。事实上定位类属性设置到父元素上，均不会影响父元素内浮动的元素。但是父元素内元素如果设置了 display：inline-block，则对父元素设置一些定位属性会影响到子元素。（这还是因为浮动元素脱离文档流的关系）。

3、垂直对齐（Vertical alignment）：inline-block元素沿着默认的基线对齐。浮动元素紧贴顶部。你可以通过vertical属性设置这个默认基线，但对浮动元素这种方法就不行了。这也是我倾向于inline-block的主要原因。

4、空白（Whitespace）：inline-block包含html空白节点。如果你的html中一系列元素每个元素之间都换行了，当你对这些元素设置inline-block时，这些元素之间就会出现空白。而浮动元素会忽略空白节点，互相紧贴

IE6和IE7：Ie67对此属性部分支持。如果你要兼容这些浏览器，必须解决这个问题。这不是个大问题，但值得留意一下。
　　

 ### 关于第4条空白节点的解决方案

1、删除html中的空白：不要让元素之间换行，这可能比较蛋疼，但也是一种方法，特别是你元素不多的时候。

2、使用负边距：你可以用负边距来补齐空白。但你需要调整font-size，因为空白的宽度与这个属性有关系。我认为是0.25em，但我不确定。如果有人知道可以留言告诉我。

3、给父元素设置font-size:0：不管空白多大，由于空白跟font-size的关系，设置这个属性即可把空白的宽度设置为0.在实际使用的时候，你还需要给子元素重新设置font-size。