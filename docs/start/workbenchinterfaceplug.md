# 在 MainWindow 中添加interface 入口

1.在 windows/MainWindowManager.py 中的setup函数中进行 Interface 窗口实例化。如下是对PyFai积分interface 进行实例化的示例：

```python
from windows.interface.Integration.CalculatorManager import CalculatorManager
self.interfaces_calculator = CalculatorManager(self)
```

2.在create_interfaces_action中，参考 PyFai 积分 interface 添加一个按钮连接到show函数即可。

```python
create_action(
                self,
                "calculator",
                on_triggered=self.interfaces_calculator.show,
                shortcut_context=Qt.ApplicationShortcut,
            ),

```
