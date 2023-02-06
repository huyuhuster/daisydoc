# 表格

表格是向用户显示一组相关选项的有效方法。要将表小部件添加到GUI，需要将以下行添加到视图的\__init__函数:

```
self.table = QtWidgets.QTableWidget(self)
self.table.setRowCount(2)
self.table.setColumnCount(2)
grid.addWidget(self.table)
```

第一行创建widget。第二行和第三行决定表的大小。第四行将其添加到布局中。要将(不可编辑的)标签添加到表中，需要以下代码:

```python
text = QtWidgets.QTableWidgetItem(("test"))
text.setFlags(QtCore.Qt.ItemIsEnabled)
row = 0
col = 0
self.table.setItem(row, col, text)

row = 1
text2 = QtWidgets.QTableWidgetItem(("another test"))
text2.setFlags(QtCore.Qt.ItemIsEnabled)
self.table.setItem(row, col, text2)

row = 0
col = 1
self.table.setCellWidget(row, col, self.combo)
row = 1
self.table.setCellWidget(row, col, self.spin)
```

第一行创建了一个带有标签test的widget，第二个标志确保用户不能编辑该值。使用setItem函数将标签添加到表中。

表的一个有用特性是，它们可以在一个单元格中包含一个小部件。上面代码的最后五行将ComboBox和 spin box添加到表中。需要注意的是，widget现在只出现在表中。
