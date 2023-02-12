# 基于 Daisy workbench 开发 interface

Daisy workbench 的代码结构:

```shell
├── api/
├── applications/
├── framework/
├── utils/
├── venv/
├── windows/
├── algorithms_list.yaml
├── daisy_stacktrace.txt
├── data_visualization_rc.py
├── envir
├── log.txt
├── main.py
├── README.md
├── requirements.txt
├── start.py
├── start.pyc
```

api目录实现了Daisy算法调用的简单API。
framework目录存放Daisy框架。
utils目录为常用工具类集合。
venvm目录包含了软件运行环境。
windows 目录包含了 Daisy workbench 的所有组件。其中 widgets目录下面为主界面的各个组件。interface目录下面包含了各个方法学专用interface。
