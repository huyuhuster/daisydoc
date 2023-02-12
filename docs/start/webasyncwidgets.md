# 异步Widgets 

本节涵盖了两种场景，在这两种场景中，我们希望与小部件相关的代码运行时不会阻塞内核对其他执行请求的操作:

- 暂停代码，等待用户与前端的小部件交互
- 在后台更新一个小部件



**等待用户交互**

​       您可能希望暂停您的Python代码，以等待一些用户与来自前端的小部件的交互。通常这很难做到，因为运行Python代码会阻塞任何来自前端的小部件消息，直到Python代码完成。这里将通过两种方法实现这一点:使用事件循环集成，以及使用普通生成器函数。

**事件循环继承(Event loop integration)**

如果利用IPython提供的事件循环集成，那么使用Python 3中的async/await语法就可以得到一个很好的解决方案。首先，调用asyncio事件循环。这需要 ipykernel 4.7或更高版本。

```
%gui asyncio
```

然后定义一个新函数，该函数在小部件属性更改时返回一个future值。

```
import asyncio
def wait_for_change(widget, value):
    future = asyncio.Future()
    def getvalue(change):
        # make the new value available
        future.set_result(change.new)
        widget.unobserve(getvalue, value)
    widget.observe(getvalue, value)
    return future
```

最后我们回到函数，等待小部件的变化。我们将做10个单元的工作，在每一个单元之后暂停，直到我们观察到小部件中有变化。注意，小部件的值对我们是可用的，因为它是wait_for_change未来的结果。运行此函数，并改变滑块10次。

```
from ipywidgets import IntSlider, Output
slider = IntSlider()
out = Output()

async def f():
    for i in range(10):
        out.append_stdout('did work ' + str(i) + '\n')
        x = await wait_for_change(slider, 'value')
        out.append_stdout('async function continued with value ' + str(x) + '\n')
asyncio.ensure_future(f())

slider
```

```
out
```

**生成器方法(Generator approach)**

如果不能利用async/await语法，或者不想修改事件循环，也可以使用生成器函数。首先，我们定义一个装饰器，它将生成器函数与小部件更改事件挂钩。

```
from functools import wraps
def yield_for_change(widget, attribute):
    """Pause a generator to wait for a widget change event.

    This is a decorator for a generator function which pauses the generator on yield
    until the given widget attribute changes. The new value of the attribute is
    sent to the generator and is the value of the yield.
    """
    def f(iterator):
        @wraps(iterator)
        def inner():
            i = iterator()
            def next_i(change):
                try:
                    i.send(change.new)
                except StopIteration as e:
                    widget.unobserve(next_i, attribute)
            widget.observe(next_i, attribute)
            # start the generator
            next(i)
        return inner
    return f
```

然后我们设置我们的生成器。

```
from ipywidgets import IntSlider, VBox, HTML
slider2=IntSlider()

@yield_for_change(slider2, 'value')
def f():
    for i in range(10):
        print('did work %s'%i)
        x = yield
        print('generator function continued with value %s'%x)
f()

slider2
```

上述两种方法都等待小部件更改事件，但可以修改为等待其他事情，例如按钮事件消息(如“Continue”按钮)等。



**在后台更新一个小部件**

有时，您希望在后台更新小部件，以允许内核也处理其他执行请求。我们可以用线程来做这个。在下面的例子中，进度条将在后台更新，并允许主内核进行其他计算。

```python
import threading
from IPython.display import display
import ipywidgets as widgets
import time
progress = widgets.FloatProgress(value=0.0, min=0.0, max=1.0)

def work(progress):
    total = 100
    for i in range(total):
        time.sleep(0.2)
        progress.value = float(i+1)/total

thread = threading.Thread(target=work, args=(progress,))
display(progress)
thread.start()
```
