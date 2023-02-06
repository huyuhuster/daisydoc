# Matplotlib 和 MVC

创建一个允许用户操作正弦波图形的GUI的下一步是生成图形本身。

此处需要用户熟悉Matplotlib，如果不熟悉，请参阅Matplotlib文档。

Matplotlib函数可以被认为是一个model或一个view，对于是哪个没有正确的答案。它可以是一个model，因为它对数据做了一些事情，但是它也可以被认为是一个view，因为(在这种情况下)它纯粹是作为一个视觉表示来使用的。这种模糊性就是为什么MVC只是一种模式，而不是一套规则。在这种情况下，我们决定它应该是一种view。

这个view将与练习中的view一起存在。我们需要换个名字，比如 PlotView。

视图有以下import:

```python
from qtpy import QtWidgets, QtCore, QtGui
import matplotlib.pyplot as plt

from matplotlib.backends.backend_qt5agg import FigureCanvasQTAgg as FigureCanvas
```

​        第4行导入Matplotlib，最后一行允许它与GUI交互。PlotView类如下所示，它包含向图形添加数据和创建空图形(没有数据)的方法。

```python
class PlotView(QtWidgets.QWidget):
    def __init__(self, parent=None):
        super().__init__(parent)

        self.figure = plt.figure()
        grid = QtWidgets.QVBoxLayout(self)
        self.draw()
        self.canvas = self.getWidget()
        grid.addWidget(self.canvas)
        self.setLayout(grid)

    def draw(self):
        ax = self.figure.add_subplot(111)
        ax.clear()
        ax.set_xlim([0.0, 10.5])
        ax.set_ylim([-1.05, 1.05])
        ax.set_xlabel("time ($s$)")
        ax.set_ylabel("$f(t)$")
        return ax

    def getWidget(self):
        return FigureCanvas(self.figure)

    def addData(self, xvalues, yvalues, grid_lines, colour, marker):
        ax = self.draw()
        ax.grid(grid_lines)
        ax.plot(xvalues, yvalues, color=colour, marker=marker, linestyle="--")
        self.canvas.draw()
```

draw方法在没有任何数据的情况下创建plot区域。小部件是从getWidget函数获得的。最后一个方法将数据添加到plot区域，self.canvas.draw()更新plot区域，使其包含数据。
