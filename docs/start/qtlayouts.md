# Layouts

在前面的任务中，一个标签被添加到视图中。然而，标签出现在按钮下方。把标签放在按钮旁边是明智的。这是可能的使用布局。

到目前为止，我们使用了垂直布局(QtWidgets.QVBoxLayout)，现在我们将使用水平布局。可以向布局中添加子布局，这里我们将通过向垂直布局中添加水平布局来实现。小部件添加到布局中的顺序将决定它们的位置。

在视图中，我们将用以下代码替换\__init__:

```python
def __init__(self, parent=None):
    super().__init__(parent)

    self.button = QtWidgets.QPushButton('Hi', self)
    self.button.setStyleSheet("background-color:lightgrey")

    # connect button to signal
    self.button.clicked.connect(self.btn_click)
    self.label = QtWidgets.QLabel()
    self.label.setText("Button")

    # add widgets to layout
    self.sub_layout = QtWidgets.QHBoxLayout()
    self.sub_layout.addWidget(self.label)
    self.sub_layout.addWidget(self.button)

    grid = QtWidgets.QVBoxLayout(self)
    grid.addLayout(self.sub_layout)

    # set the layout for the view widget
    self.setLayout(grid)
```
