# widgets 介绍

widgets 是事件 python 对象在浏览器中的一个表示，通常作为一个控件，如滑动条、文本框等。用户可以使用小部件为jupyter notebook 构建交互式 GUI。用户还可以使用 widgets 在Python和JavaScript 之间同步有状态和无状态信息。

**使用 widgets**

使用 widgets 框架需要 导入 ipywidgets：

```
import ipywidgets as widgets
```

**显示**

​        小部件有自己的显示 **repr**，允许使用 IPython 的显示框架来显示它们。构造并返回一个 IntSlider 会自动显示小部件(如下所示)。小部件显示在代码单元格下方的输出区域中。清除单元格输出也会删除小部件。

![interslide](C:\HUYU_dir\work\HEPS\Daisy\文档\picture\interslide.png)

也可以通过 display(...) 来显示 widgets：

![interslide_display](C:\HUYU_dir\work\HEPS\Daisy\文档\picture\interslide_display.png)

如果显示同一个小部件两次，则前端中显示的实例将保持同步。小部件在后端由单个对象表示。每次显示小部件时，都会在前端创建该对象的新表示形式。这些表示称为视图。

<img src="https://ipywidgets.readthedocs.io/en/latest/_images/WidgetModelView.png" style="zoom:50%;" />

可以通过调用 close() 方法来关闭widgets。

***widgets 属性***

所有的 IPython widgets 共享相似的命名方案。比如可以通过查询 value 属性来读取 widgets 的值。

```
w = widgets.IntSlider()
w.value
```

类似的，可以通过设置 widgets 的 value 属性来设置它的值。

```
w.value = 100
```

除了value 外，大部分 widgets 共享 keys`，`description`, 和 `disabled`属性。可以通过查询 keys 属性来查看任何特定 widgets 的同步，有状态属性的完整列表。

```
[1]:w.keys
[1]:['_dom_classes',
    '_model_module',
    '_model_module_version',
    '_model_name',
    '_view_count',
    '_view_module',
    '_view_module_version',
    '_view_name',
    'behavior',
    'continuous_update',
    'description',
    'description_allow_html',
    'disabled',
    'layout',
    'max',
    'min',
    'orientation',
    'readout',
    'readout_format',
    'step',
    'style',
    'tabbable',
    'tooltip',
    'value']
```

当创建一个 widgets 时，可以在 widgets 构造函数中以关键字参数的形式定义某些或者所有的初始值。

```
widgets.Text(value='Hello World!', disabled=True)
```

**连接两个widgets**

如果想要以两种不同方式访问相同的数值，则必须使用两种不同的widgets。用户可以使用 link 或者 jslink 方法将两个属性连接在一起来代替手动同步两个 widgets的值。如下所示，两个 widgets的value 属性被连接在一起。

```
a = widgets.FloatText()
b = widgets.FloatSlider()
display(a,b)

mylink = widgets.jslink((a, 'value'), (b, 'value'))
```

在连接的对象上调用 .unlink 来解除两个widgets之间的link。

