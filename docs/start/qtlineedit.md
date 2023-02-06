# 添加一个LineEdit

有时用户需要输入一些信息。最通用的方法是使用LineEdit，它允许用户输入任意文本。向视图的\__init__函数添加以下代码将添加一行编辑:

```python
self.text = QtWidgets.QLineEdit()
grid.addWidget(self.text)
```

在LineEdit中可以有一个默认值。也可以使用户无法修改LineEdit(对表有用)。

在使用行编辑之前要小心，因为它会给用户太多的自由。如果你知道输入是整数，那么spin box更好。如果可能的选项是有限的，那么 combo box 将是一个更好的选择。
