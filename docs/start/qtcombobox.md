# 添加一个 ComboBox

当用户可以从有限的选项列表中进行选择时，ComboBox非常有用。如果使用的是LineEdit，那么打字错误将阻止GUI正确工作;ComboBox避免了这种可能性。
下面的代码应该添加到视图的\__init__函数中。

```python
self.combo = QtWidgets.QComboBox()
options = ["one", "two", "three"]
self.combo.addItems(options)
grid.addWidget(self.combo)
```

第一行创建一个ComboBox小部件。第二行定义将在ComboBox中显示的选项，第三行将选项添加到ComboBox中。最后一行将其添加到布局中。
