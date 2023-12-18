# ROCm 生态 适配

## 1 基础简介
DTK（DCU Toolkit）是 DCU 硬件平台开发工具包。借助 DTK 开发人员可以在 DCU 上
开发、优化和部署应用。DTK 同时提供 HIP 和 CUDA 两种编程模型支持，可以快速的将
CUDA 和 ROCm 生态中的已有加速器应用部署在 DCU 上。目前，最新 DTK 已经支持多种
计算框架，包括基础数学算子、科学计算、深度学习等众多领域。

### 1.1 DTK CUDA 使用方法
如上文所述，DTK 支持 CUDA 生态。以 dtk 23.04 为例，其兼容的 CUDA 版本为 10.2.86。
利用这种特性，开发人员可以在不修改源码以及 CMakeLists.txt 的情况下，直接将 CUDA 源
码编译为 DTK 平台的可执行程序。提升了系统兼容性的同时，也为程序的移植工作提供极
大的便利（您可以理解为这是一层封装，本质上调用的是 HIP 编程模型）。
使用前需要配置相应环境变量。DTK 的 cuda 目录中提供了设置环境变量的脚本 env.sh，
设置方法如下：
1） 首先加载 DTK 目录下的 env.sh，用于设置 DTK 环境变量。
source env.sh
2） 接着加载 DTK 中 cuda 目录下的 env.sh，用于设置 CUDA 环境变量。
source $ROCM_PATH/cuda/env.sh
加载上述环境后，可以直接在 DCU 上构建安装支持 CUDA 加速的应用（使用 CUDA
构建的方法）。当然，CUDA 的部分功能目前 DTK 还不支持，需要进行修改。

