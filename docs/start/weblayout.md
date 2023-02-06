# Jupyter widgets的布局和样式

本节介绍如何布局和样式化木星交互小部件，以构建丰富的、响应式的基于小部件的应用程序。

## layout 属性
jupiter交互小部件有一个layout属性，它暴露了许多影响小部件布局方式的CSS特性。

**暴露 CSS 特性**

下面的属性映射到相同名称的CSS特性的值(下划线被破折号取代)，应用到相应小部件的顶部DOM元素。

**Sizes**

- `height`
- `width`
- `max_height`
- `max_width`
- `min_height`
- `min_width`

**Display**

- `visibility`
- `display`
- `overflow`

**Box model**

- `border`
- `margin`
- `padding`

**Positioning**

- `top`
- `left`
- `bottom`
- `right`

**Flexbox**

- `order`
- `flex_flow`
- `align_items`
- `flex`
- `align_self`
- `align_content`
- `justify_content`

**Grid layout**

- `grid_auto_columns`
- `grid_auto_flow`
- `grid_auto_rows`
- `grid_gap`
- `grid_template`
- `grid_row`
- `grid_column`

**简写 CSS 特性**

​       某些CSS特性，如margin-[top/right/bottom/left]似乎缺失了。padding-[top/right/bottom/left]也是一样。事实上，可以通过margin属性单独指定[top/right/bottom/left]边距，通过传递字符串'100px 150px 100px 80px'分别表示上、右、下和左边距100、150、100和80像素。类似地，flex属性可以保存flex-grow、flex-shrink和flex-basis的值。border属性是border-width、border-style(必需的)和border-color的简写属性。

**简单示例**

下面的例子展示了如何调整一个按钮的大小，使其视图的高度为80px，宽度为可用空间的50%:

![layout1](C:\HUYU_dir\work\HEPS\Daisy\文档\picture\layout1.png)

layout属性可以在多个小部件之间共享并直接分配。

![layout2](C:\HUYU_dir\work\HEPS\Daisy\文档\picture\layout2.png)

**描述 （Description）**

因为description长度在默认情况下是固定的，所以较长的description被截断了。可以更改description的长度以适合description文本。然而，这将使小部件本身更短。可以通过使用小部件的样式调整description宽度和小部件宽度来更改这两者。如果需要更灵活地布局小部件和描述，可以直接使用Label小部件。

**Natural sizes, and arrangements using HBox and VBox**

大多数核心部件都有默认的高度和宽度，可以很好地平铺在一起。这允许基于HBox和VBox助手功能的简单布局自然对齐.

**Latex**

滑动条和文本输入等小部件有一个描述属性，可以呈现Latex equation。Label小部件也显示Latex方程。

**数字格式**

滑块有一个读出字段，可以使用Python的格式规范迷你语言进行格式化。如果用于读出的可用空间对于滑块值的字符串表示来说太窄，则应用不同的样式来表明并非所有数字都可见。

**Flexbox 布局**

​        上面的HBox和VBox类是Box小部件的特殊情况。Box小部件支持整个CSS flexbox规范和Grid布局规范，在jupiter笔记本中支持丰富的响应式布局。它的目的是提供一种有效的方式来布局、对齐和分配容器中的物品之间的空间。同样，整个flexbox规范是通过容器部件(Box)的布局属性和所包含的项公开的。可以在所有包含的条目中共享相同的布局属性。

​        关于flexbox布局的教程遵循了Chris Coyier的文章《[flexbox完整指南](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)》中的内容。

**预定义样式**

如果希望小部件的样式使用环境定义的颜色和样式(与例如笔记本主题一致)，许多小部件都支持在预定义样式列表中进行选择。例如，Button小部件有一个button_style属性，它可以有5个不同的值:

- `'primary'`
- `'success'`
- `'info'`
- `'warning'`
- `'danger'`

** `style` 属性**
虽然layout属性只暴露了小部件顶级DOM元素与布局相关的CSS属性，但style属性用于暴露小部件的非布局相关的样式属性。但是，style属性的属性是特定于每个小部件类型的。可以通过keys属性获得小部件的style属性列表。就像layout属性一样，小部件样式也可以分配给其他小部件。小部件样式属性特定于每个小部件类型。样式可以在构建小部件时给出，可以作为一个特定的Style实例，也可以作为一个字典。

完整的Jupyter widgets 布局和样式说明请参考：
https://ipywidgets.readthedocs.io/en/latest/examples/Widget%20Styling.html#

