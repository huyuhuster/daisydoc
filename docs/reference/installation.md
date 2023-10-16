# Installation

目前，`Daisy`暂时仅提供源码编译安装的方式，面向`Linux`平台。

`Daisy`源码托管于高能所**git**平台，[code.ihep.ac.cn](https://code.ihep.ac.cn)，项目地址是 [Daisy Project](https://code.ihep.ac.cn/hepscc/Daisy)。

编译安装过程如下：

1. 从仓库复制源代码
   ```bash
   git clone https://code.ihep.ac.cn/hepscc/Daisy.git
   ```
   或直接下载压缩包，
   ```bash
   wget https://code.ihep.ac.cn/hepscc/Daisy/-/archive/main/Daisy-main.tar.gz
   ```

2. 进入代码所在根目录（压缩包请先解压缩），执行：
   ```bash
   . ./setup.sh
   ```

`Daisy`将自动完成编译、安装和设置好相应的环境变量，之后每次使用`Daisy`，都可以执行此脚本；并且脚本经过优化，不会发生重复编译。

更新`Daisy`项目时，请保存好自己的工作内容，重新执行上述两步过程即可。

欢迎大家提**issues**和贡献**commits**！
