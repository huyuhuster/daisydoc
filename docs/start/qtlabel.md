# 添加一个 Label

在本节中，我们将向GUI添加一个标签。标签是用户不能编辑的文本显示。
要添加标签，我们需要向视图添加三行。第一行创建标签小部件，第二行分配标签的值(它将显示的内容)，最后一行将其添加到视图的布局中。

```python
self.label = QtWidgets.QLabel()
self.label.setText("Button")
grid.addWidget(self.label)
```

这段代码应该添加到视图的构造函数中(即它的\__init__函数)。
