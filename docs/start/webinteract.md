# 使用Interact

交互函数(ipywidgets. interactive)自动创建用户界面(UI)控件，以交互方式查看代码和数据。这是开始使用IPython的小部件的最简单的方法。

**Basic `interact`**

在最基本的层次上，interact 自动生成函数参数的UI控件，然后在交互操作控件时使用这些参数调用函数。要使用interact，需要定义一个想要研究的函数。这里是一个返回唯一参数x的函数。

```python
def f(x):
    return x
```

当将此函数作为第一个参数与整数关键字参数(x=10)交互时，将生成一个滑块并绑定到函数参数。

```
interact(f, x=10);
```

当移动滑块时，将调用函数，并打印其返回值。

如果传递True或False，交互将生成一个复选框:

```
interact(f, x=True);
```

如果传递一个字符串，交互将生成一个文本框。

```
interact(f, x='Hi there!');
```

交互也可以用作装饰器。这允许你定义一个函数并在一个镜头中与它交互。如本例所示，还可以使用具有多个参数的函数进行交互。

```python
@interact(x=True, y=1.0)
def g(x, y):
    return (x, y)
```

**Widget abbreviations **

当传递一个整值关键字参数10 (x=10)到interact时，它会生成一个范围为[-10，+3*10]的整值滑块控件。在本例中，10是一个实际滑块小部件的缩写。事实上，如果我们传递这个IntSlider作为x的关键字参数，也可以得到相同的结果:

```
IntSlider(min=-10, max=30, step=1, value=10)
interact(f, x=widgets.IntSlider(min=-10, max=30, step=1, value=10));
```

下表给出了不同参数类型的概述，以及它们如何映射到交互控件:

| Keyword argument                                             | Widget      |
| ------------------------------------------------------------ | ----------- |
| True` or `False                                              | Checkbox    |
| 'Hi there'                                                   | Text        |
| `value` or `(min,max)` or `(min,max,step)` if integers are passed | IntSlider   |
| `value` or `(min,max)` or `(min,max,step)` if floats are passed | FloatSlider |
| `['orange','apple']` or `[(‘one’, 1), (‘two’, 2)]            | Dropdown    |

注意，如果给出了一个元组列表(表示离散的选择)，就会使用下拉菜单，如果给出了一个元组(表示一个范围)，就会使用滑动条。
