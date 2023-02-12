# mock 示例

在这个示例中，将模拟controller练习中的controller。所有的方法都应该被模拟，只有在必要的时候才应该显示返回值(在这种情况下，返回值并不重要)。应该测试updatePlot函数，以确保它调用了正确的方法。具体实现如下：

```python
import sys
import controller
import view

import unittest
from unittest import mock

class ControllerTest(unittest.TestCase):
    def setUp(self):
        self.view = mock.create_autospec(view.View)

        # mock view
        self.view.plotSignal = mock.Mock()
        self.view.getColour = mock.Mock(return_value="black")
        self.view.getGridLines =mock.Mock(return_value=True)
        self.view.getFreq =mock.Mock(return_value=3.14)
        self.view.getPhase =mock.Mock(return_value=0.56)
        self.view.buttonPressed = mock.Mock()
        self.view.setTableRow = mock.Mock()
        self.view.addWidgetToTable = mock.Mock()
        self.view.addITemToTable = mock.Mock()

        self.controller = controller.Controller(self.view)

    def test_updatePlot(self):
        self.controller.updatePlot()
        self.view.getColour.assert_called_once()
        self.view.getGridLines.assert_called_once()
        self.view.getFreq.assert_called_once()
        self.view.getPhase.assert_called_once()

if __name__ == "__main__":
    unittest.main()
```
