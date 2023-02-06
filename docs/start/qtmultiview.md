# 多重视图

 在其他MVC模式中有一个MVC模式是可能的。如果每个完整的MVC模式都被认为是一个小部件，那么将一个MVC模式嵌入另一个MVC模式实际上就是添加另一个小部件。这对于创建可在多个gui中使用的多功能小部件非常有用。
​        我们将把练习中的View小部件和前一节中的PlotView小部件组合成一个视图。为了实现这一点，我们将创建一个“main”视图:

```python
from qtpy import QtWidgets, QtCore, QtGui

import numpy as np
import plot_view
import view

class MainView(QtWidgets.QWidget):

    def __init__(self, parent=None):
        super().__init__(parent)

        grid = QtWidgets.QVBoxLayout(self)
        self.plot_view = plot_view.PlotView(parent=self)
        self.options_view = view.View(parent=self)

        grid.addWidget(self.plot_view)
        grid.addWidget(self.options_view)
        self.setLayout(grid)
```

这里需要注意的重要事情是，当PlotView和View被创建时，父对象被设置为MainView。

main只需要导入main_view:

```pytho
class Demo(QtWidgets.QMainWindow):
    def __init__(self, parent=None):
        super().__init__(parent)

        self.window = QtWidgets.QMainWindow()
        my_view = main_view.MainView()

        # set the view for the main window
        self.setCentralWidget(my_view)
        self.setWindowTitle("view tutorial")
```

你可能会注意到这个main没有包含controller。现在我们已经将两个视图嵌入到MainView中，controller也应该以相同的方式拆分。
