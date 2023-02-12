# 添加一个 Spin Box

Spin Box 是确保用户只选择整数值的一种有用方法。用户可以在Spin Box中输入一个整数或使用箭头增加该值。也可以对Spin Box施加约束，例如设置最大值。

```python
self.spin = QtWidgets.QSpinBox()
grid.addWidget(self.spin)
```

要向GUI添加Spin Box，需要将上述代码添加到视图的\__init__函数中。
