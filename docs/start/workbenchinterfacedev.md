# Interface 开发

本例通过开发一个求矩阵最大值最小值的interface为例。该小窗口的功能包括：1.调用Daisy算法从HDF5文件中读取矩阵数据；2.调用 Daisy算法求矩阵数据的最大值或最小值；3.打印结果。 该interface的代码目录如下：

```shell
├── CalculatorManager.py
├── Controller.py
├── __init__.py
├── Model.py
└── View.py
```

CalculatorManager.py 文件为该interface的入口。其代码如下：

```python
from PyQt5.QtCore import *
import PyQt5.QtWidgets as QtWidgets

import sys

from windows.interface.Calculator.Model import Model
from windows.interface.Calculator.View import View
from windows.interface.Calculator.Controller import Controller

"""
A wrapper class for setting the main window of the interface
"""
class CalculatorManager(QtWidgets.QMainWindow):

    def __init__(self, parent=None):
        super(CalculatorManager, self).__init__(parent)  # noqa
        self.setWindowFlag(Qt.Window)
        self.setAutoFillBackground(True)

        self.window = QtWidgets.QMainWindow()
        demo_view = View()
        demo_model = Model()
        # create controller
        self.controller = Controller(demo_view, demo_model)
        # set the view for the main window
        self.setCentralWidget(demo_view)
        self.setWindowTitle("Calculator")

    def show(self) -> None:
        super(CalculatorManager, self).show()
        self.activateWindow()
```

View.py 文件定义了interface的外观，本例的界面如下图所示，为一个包含5个项目的table。前面四个项目为输入项目，分别为：输入文件选择、h5文件内的数据路径、算子选择（max/min）、展示类型选择（打印在屏幕上/更新计算结果/打印和更新），最后一个项目为计算结果。

![image-20220728180848011](C:\Users\huyuh\AppData\Roaming\Typora\typora-user-images\image-20220728180848011.png)

View.py 的代码如下：

```python
import PyQt5.QtCore as QtCore
import PyQt5.QtWidgets as QtWidgets
import os

class View(QtWidgets.QDialog):
    # In PyQt signals are the first thing to be defined in a class:
    displaySignal = QtCore.pyqtSignal()
    btnSignal = QtCore.pyqtSignal()

    def __init__(self, parent=None):
        # Call QDialog's constructor
        super(View, self).__init__(parent)

        # Initialise the widgets for the view (this can also be done from Qt Creator
        self.table = QtWidgets.QTableWidget()
        self.table.setWindowTitle("MVP Demo")
        self.table.resize(600, 250)
        self.table.setRowCount(5)
        self.table.setColumnCount(2)
        self.table.setHorizontalHeaderLabels("name;value;".split(";"))

        self.FilePath = ''

        # Set display values in the widgets
        keys = ['File path', 'Date Path', 'operation',  'display', 'result']
        self.combo = {}
        self.create_File_selction(0, 1, 'File')
        self.create_combo_table(2, 1, 'operations')
        self.create_combo_table(3, 1, 'display')
        for row in range(len(keys)):
            self.set_names(keys[row], row)

        # Initialise layout of the widget and add child widgets to it
        grid = QtWidgets.QGridLayout()
        grid.addWidget(self.table)

        self.button = QtWidgets.QPushButton('Calculate', self)
        self.button.setStyleSheet("background-color:lightgrey")
        grid.addWidget(self.button)

        # Connect button click handler method to the button's 'clicked' signal
        self.button.clicked.connect(self.btn_click)
        # connect method to handle combo box selection changing to the corresponding signal
        self.combo['display'].currentIndexChanged.connect(self.display_changed)
        self.file.clicked.connect(self.msg)

        # Set the layout for the view widget
        self.setLayout(grid)

    # The next two methods handle the signals connected to in the presenter
    # They emit custom signals that can be caught by a presenter
    def btn_click(self):
        self.btnSignal.emit()

    def display_changed(self):
        self.displaySignal.emit()

    # Populate view
    def create_combo_table(self, row, col, key):
        self.combo[key] = QtWidgets.QComboBox()
        options = ['test']
        self.combo[key].addItems(options)
        self.table.setCellWidget(row, col, self.combo[key])

    def create_File_selction(self, row, col, key):
        self.file = QtWidgets.QPushButton()
        self.file.setGeometry(QtCore.QRect(57, 660, 175, 28))
        self.file.setObjectName("file")
        self.file.setStyleSheet("background-color:rgb(111,180,219)")
        self.combo[key] = self.file
        self.table.setCellWidget(row, col, self.combo[key])

    # The next 5 methods update the appearance of the view.

    def set_options(self, key, options):
        self.combo[key].clear()
        self.combo[key].addItems(options)

    def set_names(self, name, row):
        text = QtWidgets.QTableWidgetItem(name)
        text.setFlags(QtCore.Qt.ItemIsEnabled)
        self.table.setItem(row, 0, text)

    # Update the view with the result of a calculation
    def setResult(self, value):
        self.table.setItem(4, 1, QtWidgets.QTableWidgetItem(str(value)))

    def hide_display(self):
        self.table.setRowHidden(4, True)

    def show_display(self):
        self.table.setRowHidden(4, False)

    # Finally, we have the get methods to allow the Controller to read the user's input

    def get_value(self, row):
        return self.table.item(row, 1).text()

    def get_operation(self):
        return self.combo['operations'].currentText()

    def get_display(self):
        return self.combo["display"].currentText()

    def msg(self,Filepath):
        m = QtWidgets.QFileDialog.getOpenFileName(self, "选取文件", os.getcwd(),"HDF5 File (*.h5);;")
        self.file.setText(m[0])
        self.FilePath = m[0]                              
```

Model.py 文件包含了具体的算法逻辑，Daisy所有的算法以及其他可能需要执行的计算均在Model中运行。Model.py 的代码如下：

```python
from api import work_flow

class Model(object):
    def __init__(self):
        # Get the algorithms from Daisy workflowengine
        self.MaxMin = work_flow.engine['MaxMin']
        self.Loadh5 = work_flow.engine['LoadHDF5']

    def CalMaxmin(self, Filepath, DataPath, operation):
        # Load matrix data from the HDF5 file
        self.Loadh5.config({'inputfile_name': Filepath})
        self.Loadh5.execute(input_path = DataPath, output_dataobj='data_h5')
        # Calculate the maximum or minimum of the matrix
        if operation == "max":
            self.MaxMin.execute(input_dataobj='data_h5',  output_dataobj='maxmin', showMin=False)
        elif operation == "min":
            self.MaxMin.execute(input_dataobj='data_h5',  output_dataobj='maxmin', showMin=True)
        # Get the result from the datastore     
        self.result = work_flow.engine.datastore['maxmin']
        return self.result
```

Controller 充当“中间人”。它从 View 接收数据，将其传递给Model进行处理，从Model接收返回的数据，并将其传递给View，以显示给用户。Controller 通常应该包含相对简单的逻辑。Model和View为Controller类的成员，在初始化时传递给Controller。Controller.py的代码如下：

```python
from PyQt5 import  QtWidgets

class Controller(object):
    # Pass the view and model into the presenter
    def __init__(self, demo_view, demo_model):
        self.model = demo_model
        self.view = demo_view

        # Define the initial view
        # Note that, in the view, the drop-down could be replaced with a set of
        # tick boxes and this line would remain unchanged - an advantage of
        # decoupling the presenter and view
        self.view.set_options('operations', ['max', 'min'])
        self.view.set_options('display', ['print', 'update', 'print and update'])
        self.printToScreen = True
        self.view.hide_display()

        # Connect to the view's custom signals
        self.view.btnSignal.connect(self.handle_button)
        self.view.displaySignal.connect(self.display_update)

    # The final two methods handle the signals
    def display_update(self):
        display = self.view.get_display()
        if display == 'update':
            self.printToScreen = False
            self.view.show_display()
        elif display == 'print':
            self.printToScreen = True
            self.view.hide_display()
        else:
            self.printToScreen = True
            self.view.show_display()

    def handle_button(self):
        # Get the values from view
        Filepath = self.view.FilePath
        DataPath = self.view.get_value(1)
        operation = self.view.get_operation()
        # The model does the hard work for us
        result = self.model.CalMaxmin(Filepath, DataPath, operation)

        if self.printToScreen:
            print(result)
        self.view.setResult(result)
```
