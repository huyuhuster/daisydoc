# 简单的 model

model 用于在GUI中做“hard sums”。在一个controller中有多个model是可能的，正如我们将在这个例子中看到的。对于这个例子，我们保持model尽量简单，实际上它们会复杂得多。

第一个模型为用户生成数据:

```python
import numpy as np

class DataGenerator(object):

    def __init__(self):
        self.x_data = np.linspace(0.0, 10.0, 100)
        self.y_data = []

    def genData(self, freq, phi):
        self.y_data = np.sin(freq * self.x_data + phi)

    def getXData(self):
        return self.x_data

    def getYData(self):
        return self.y_data
```

​        model方法可以分为三种类型:初始化、计算按钮和获取方法。

​        在这种情况下，我们有一个独特的第二种方法。通常，它将被放置到自己的文件中，但是为了简单起见，我们将把它包含在与上述代码相同的文件中。
