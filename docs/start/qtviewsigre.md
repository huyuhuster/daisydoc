# 从view接收信号

 在Add Button部分中，我们有对视图中按钮按下的响应。实际上，这并不是一个好的实现。如果响应更复杂，那么就很难维护视图，因为它会变得非常长。此外，创建GUI的外观是相当简单的，任何逻辑/响应都应该包含在controller 中。

​        在本节中，我们将为按钮被按下时制作一个简单的controller 。首先，我们将从View开始:

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
        # set the layout for the view widget
        self.setLayout(grid)

    #send signals
    def btn_click(self):
        print ("hellow from view")
        self.doSomethingSignal.emit()
```

​        上面的代码增加了两个新功能。第一个是在第8行创建一个自定义信号。也可以传递带有信号的对象。第二个新增功能是btn_click现在发出自定义信号，并将被controller 捕获。

​        controller 用View初始化，并且必须是controller 类的成员。因此，可以通过传递一个不同的View给演示者来改变视图。例如，您可能希望将小部件放在网格或表格中。controller 将自定义信号从View连接到它自己的函数(handleButton)。

```python
class controller (object):

    # pass the view and model into the controller 
    def __init__(self, view):
        self.view = view

        self.view.doSomethingSignal.connect(self.handleButton)

    # handle signals
    def handleButton(self):
        print("hello world, from the presenter")
```

main函数如下：

```python
from qtpy import QtWidgets, QtCore, QtGui

import sys
import view
import presenter


"""
A wrapper class for setting the main window
"""
class Demo(QtWidgets.QMainWindow):
    def __init__(self, parent=None):
        super().__init__(parent)

        self.window = QtWidgets.QMainWindow()
        my_view = view.View(self)
        self.my_presenter = presenter.Presenter(my_view)
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

View和 controller 都被创建，但是只有controller 必须是Demo类的成员。创建View是为了传递给controller，View 很容易被替换。


