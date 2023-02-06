# model 示例

在上一节中，我们不需要更新view。然而，当更改允许的线条颜色选项列表时，最好是更新model 。controller 应该能够更新view，只显示model中允许的线条颜色。要实现这一点，您需要向model添加:

- 一个函数，该函数包含允许的行颜色列表，并在调用时返回它们。不需要是类的方法。
- view中的一个方法，用于更新ComboBox值以匹配某些输入值。。
- 在controller 的初始化中，从model中获得允许的颜色并将它们传递给view。

该model现在应该包含以下顶级函数:

```python
def line_colours():
    colour_table = ["red", "blue", "black"]
    return colour_table
```

view应该包含以下方法:

```python
def setColours(self,options):
    self.colours.clear()
    self.colours.addItems(options)
```

controller的初始化现在应该是:

```python
def __init__(self, view, data_model, colour_list):
   self.view = view
   self.data_model = data_model

   self.view.setColours(colour_list)
   # connect statements
   self.view.plotSignal.connect(self.updatePlot)
```

Main模块现在应该将两个model传递给controller:

```
def __init__(self, parent=None):
    super().__init__(parent)

    self.window = QtWidgets.QMainWindow()
    my_view = view.View()
    data_model = model.DataGenerator()
    colour_list = model.line_colours()

    self.controller = controller.Controller(my_view, data_model, colour_list)
    # set the view for the main window
    self.setCentralWidget(my_view)
    self.setWindowTitle("view tutorial")
```
