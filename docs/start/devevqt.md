# 基于 QT 的通用用户界面及interface开发环境

基于QT的interface开发环境目前是以虚拟机的形式存在，虚拟机会提供必要的依赖软件。虚拟机的使用请联系 Daisy 管理人员。

Daisy workbench 依赖 sniper 和 Daisy，本项目提供的虚拟机已经提前安装好了，用户只需要配置相应的环境。

Daisy workbench 代码仓库位于 https://code.ihep.ac.cn/hepscc/daisy-workbench。开发者 git clone 到本地之后，需要配置运行环境，具体命令如下：

```shell
export DYLD_FALLBACK_LIBRARY_PATH={sniper_install_path}/lib
export CMAKE_PREFIX_PATH={sniper_install_path}/lib:$CMAKE_PREFIX_PATH
export SNIPER_INIT_FILE={sniper_install_path}/share/sniper/.init.json
export PYTHONPATH={sniper_install_path}/python:{daisy_path}/../:$PYTHONPATH
```

然后通过一下命令启动 Daisy workbench, 验证环境是否配置正确。

```
python {Daisyworkbench_install_path}/main.py 
```

用户需要将开发interface所用到的算法集成到Daisy中。
