# jupyter 二次开发

## 1. Jupyterlab Launcher 定制插件安装及其环境变量配置

- 本项目提供了插件`jupyter_app_launcher`[^1]的安装包：

  * jupyter_app_launcher-0.2.2-py3-none-any.whl

  该安装包的下载地址为：[Download Link](https://docs.ihep.ac.cn/link/AA886D225778EF4C39A8B43E2A6A014C57)

  安装该插件的命令为（需要`jupyterlab>=4.0`）：

  ```bash
  pip install jupyter_app_launcher-0.2.2-py3-none-any.whl
  ```

- 该插件在菜单栏提供了**Home**菜单，可以点击菜单"Return Home"返回jupyterhub的首页；

  ![Home button](../images/jupyterhubhome.png)

  当Jupyterlab插件jupyter_app_launcher已经安装在jupyterlab开发环境之中，用户只需修改对应的配置文件，即可在jupyterlab的launcher页面添加应用图标。

  插件jupyter_app_launcher的配置文件位置为：

  * <font color=#FF0000 >/opt/jupyter_app_launcher/jp_app_launcher.yaml</font>

  指向该地址的环境变量为 **JUPYTER_APP_LAUNCHER_PATH**，用户使用以下命令使该环境变量生效：

  ```bash
  export JUPYTER_APP_LAUNCHER_PATH="/opt/jupyter_app_launcher
  ```

- 关于配置文件的书写规则如下，原文可参考[Documentation](https://jupyter-app-launcher.readthedocs.io/en/latest/usage.html).

  ```yaml
  - title: Dashboard example
    description: Example of opening a notebook in dashboard mode without Voila
    source: ../../samples/sample.ipynb
    cwd: ../../samples
    type: notebook-grid

  - title: Notebook example
    description: Example of opening a notebook in dashboard mode without Voila
    source: ../../samples/sample.ipynb
    cwd: ../../samples
    type: notebook
    catalog: Another catalog

  - title: Voila example
    description: Example of opening a notebook in dashboard mode with Voila
    source: ../../samples/sample.ipynb
    cwd: ../../samples
    type: notebook-voila
    args:
        theme: dark
    catalog: Another catalog

  - title: URL example
    description: Example of opening a URL in a tab
    source: https://jupyterlab.readthedocs.io/en/stable/
    type: url
    catalog: Another catalog
    args:
        sandbox: [ 'allow-same-origin', 'allow-scripts', 'allow-downloads', 'allow-modals', 'allow-popups']

  - title: URL example (new window)
    description: Example of opening a URL in a new browser window
    source: https://jupyterlab.readthedocs.io/en/stable/
    type: url
    catalog: Another catalog
    args:
        createNewWindow: true

  - title: Streamlit example
    description: Example of opening a streamlit app
    source: http://localhost:$PORT/
    cwd: ./
    type: local-server
    args:
      - streamlit
      - run
      - st_app.py
      - --server.headless=true
      - --server.port=$PORT
    catalog: Another catalog

  - title: Command example
    description: Example of calling JupyterLab commands
    type: jupyterlab-commands
    source:
      - label: Command 1
        id: 'filebrowser:open-path'
        args:
          path: sample.ipynb
      - label: Command 2
        id: 'filebrowser:open-path'
        args:
          path: sample-2.ipynb
    catalog: Another catalog
  ```

  配置文件书写规则如下：
  * title: launcher 页面图标的名称；
  * description: 对该应用的说明；（可选）
  * icon: 该应用的展示图标，仅支持svg格式；（可选）
  * catalog: 用于对应用分类，同一catalog将位于同一行；（可选）
  * source: 本机资源文件，例如Notebook, voila的源文件，URL， local server的链接地址；
  * type: 应用的类型，目前支持的类型如下：
  	- notebook
  	- markdown
  	- notebook-grid
  	- notebook-voila
  	- local-server
  	- url
    - jupyterlab-commands
  * args: 参数，不同的type类型，所支持的参数也不尽相同；需要特别提出的是，默认这些应用的打开都是用与Launcher并列的tab页面；对于 *notebook-voila* 和 *url* 这两类应用，可以设置 **createNewWindow**这一参数，将其设置为**true**，就可以在浏览器的新窗口（页面）中打开应用了。

- 如果修改了配置文件，则需要重启jupyter lab服务，目前无法做到实时更改实时显示。


[^1]: 感谢github项目[trungleduc/jupyter_app_launcher](https://github.com/trungleduc/jupyter_app_launcher.git)
