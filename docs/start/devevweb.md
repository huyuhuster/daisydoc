# web 数据分析平台开发环境

在浏览器地址栏输入 [http://opendata.ihep.ac.cn](http://opendata.ihep.ac.cn/)，进入登录界面，点击“Sign in with IHEPSSO”进入高能所统一认证系统界面。在统一认证系统界面中输入账号以及密码进行登录。用户登录之后进入服务选取界面。在服务选取界面中选取 ‘Daisy development’ 选项，点击页面底部 ‘Start’ 按钮进入用户 jupyterlab 界面。

​        待进入 jupyterlab 界面之后就可以看到右边的 ‘launcher’ 窗口。如果看不见可以点击左上方菜单栏下面的+号图标，新建一个。在‘launcher’窗口的other部分中点击 ‘Terminal’ 图标，进入shell界面。在shell界面中执行命令，配置环境:

```
cd ~
source /opt/setup.sh
```

在 ipython 中能成功 import Daisy 则标识环境设置成功。此时用户可以在 /Daisy 目录下开发并测试自己的算法。
