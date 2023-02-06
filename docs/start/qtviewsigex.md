# 从View提取信息

controller 需要一种从View 中获取信息的方法。这可以通过get方法完成，就像处理普通类一样。下面是一个关于如何使用 view 的get方法的简单示例。

```python
from qtpy import QtWidgets, QtCore, QtGui


class View(QtWidgets.QWidget):

    doSomethingSignal = QtCore.Signal()

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

        self.value = QtWidgets.QLineEdit()
        grid.addWidget(self.value)


        # set the layout for the view widget
        self.setLayout(grid)

    #send signals
    def btn_click(self):
        print ("hellow from view")
        self.doSomethingSignal.emit()

    def getValue(self):
        return float(self.value.text())
```

最后一个函数getValue返回LineEdit的值。由于text()返回一个字符串，因此输出被类型转换为一个浮点数。

演示者在handleButton方法中添加了以下代码:

```python
value = self.view.getValue()
print("Value is "+str(value))
```

它从View获取值，然后将其打印到屏幕。
