# 虚拟（mock） controller 

 View 应该非常简单，不需要进行测试。然而，controller 包含逻辑，因此应该进行测试。当测试controller 时，我们不想创建实际的GUI，相反，我们只想确保发出了正确的调用，这是通过mock看到unittest文档来实现的。在这里，我们就主要问题作一个简短的讨论。

首先导入声明

```python
import sys
import presenter
import view

import unittest
from unittest import mock
```

然后初始化测试类:

```python
class ControllerTest(unittest.TestCase):
    def setUp(self):
        self.view = mock.create_autospec(view.View)

        # mock view
        self.view.doSomethingSignal = mock.Mock()
        self.view.btn_click = mock.Mock()
        self.view.getValue = mock.Mock(return_value=3.14)

        self.controller  = controller.Controller(self.view)
```

Create_autospec模拟括号中包含的类。然后需要使用mock. mock显式地模拟方法。此外，当需要返回值时，将在对mock.Mock的调用中提供。

测试如下图所示:

```python
    def test_doSomething(self):
        self.Controller.handleButton()
        self.view.getValue.assert_called_once()
```

​        这里调用handleButton函数，然后使用assert_called_once来确保调用view中的方法的次数正确。这是一种健壮的方法，用于检查函数被调用了多少次。

​        也可以用self.assertEqual(1, self.view.getValue.call_count)或python assert语句。然而，使用来自mock和unittest库的更具体的断言可以使目的和错误消息更清晰。

​        最后一段代码用于执行测试:

```
if __name__ == "__main__":
    unittest.main()
```
