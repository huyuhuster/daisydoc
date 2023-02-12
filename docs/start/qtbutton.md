# 添加一个 Button

在这个任务中，创建一个包含单个按钮的GUI。

**View**

下面的代码创建了一个包含单个按钮的QWidget。当按下按钮时，它将向终端屏幕打印一条消息。

首先，我们需要导入相关的包，其中包括PyQt。

```python
from qtpy import QtWidgets, QtCore, QtGui
```

然后，我们将View类创建为一个QWidget。每个视图都有一个父视图。因此，如果父视图被销毁，视图将自动被销毁(除非视图通过对接从父视图被移除)。

```python
class View(QtWidgets.QWidget):

    def __init__(self, parent=None):
        super().__init__(parent)
```

接下来，我们创建一个布局，并添加一个按钮

```python
        grid = QtWidgets.QGridLayout()
        self.button = QtWidgets.QPushButton('Hi', self)
        self.button.setStyleSheet("background-color:lightgrey")

        # connect button to signal
        self.button.clicked.connect(self.btn_click)
        # add button to layout
        grid.addWidget(self.button)
        # set the layout for the view widget
        self.setLayout(grid)
```

上面的connect语句意味着当按钮被按下时，函数btn_click被调用:

```python
    def btn_click(self):
        print("Hello world")
```

**Main**

为了运行GUI，我们需要一个' main '模块。假设上面的内容都保存在view.py中，main.py将包含:

```python
from qtpy import QtWidgets

import sys

import view

"""
A wrapper class for setting the main window
"""
class Demo(QtWidgets.QMainWindow):
    def __init__(self,parent=None):
        super().__init__(parent)

        self.window=QtWidgets.QMainWindow()
        my_view = view.View()
        # set the view for the main window
        self.setCentralWidget(my_view)
        self.setWindowTitle("view tutorial")

def get_qapplication_instance():
    if QtWidgets.QApplication.instance():
        app = QtWidgets.QApplication.instance()
    else:
        app = QtWidgets.QApplication(sys.argv)
    return app

app = get_qapplication_instance()
window = Demo()
window.show()
app.exec_()
```
