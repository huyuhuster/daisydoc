# HEPSCT

HEPSCT 是 HEPSCC 开发的基于 web 的CT数据处理应用。包含图像导入、预处理、相位恢复（待集成）以及图像重建和数据分析等模块。能够实现CT数据的快速重建。从而满足国内外同步辐射用户对于“海量”X射线CT数据处理的需求。 

## 软件特点 

* 重建核心算法100%实现GPU加速, 利用多线程实现数据的快速读取及IO、CPU、GPU数据处理同步进行 
* 实现多尺度CT重建转轴位置的自动校正，基于小波变换进行环形伪影的去除  
* 提供FBP、Grid、EM等多种重建算法，利用Grid网格算法能够在数秒内完成对图像尺寸为2k*2k的投影数据集的三维重建计算 

HEPSCT 的前端基于Jupyterlab生态，由ipywidgets包开发。

HEPSCT 后端集成了HEPS X射线显微成像线站团队开发的MOCUPY，该软件基于CUDA以及Python编写，基于GPU加速CT图像重建。


## 软件访问方式

### 账号申请
如果没有高能所统一认证账号，请先[申请统一认证账号](http://login.ihep.ac.cn/)， 并申请AFS集群账号，隶属应用请选择 "HEPS"，用户组请选择“HEPSBL”。

### 软件访问
本软件以web页面的形式为用户提供服务，用户不需要下载和安装软件。在浏览器中输入访问地址： https://hepscompute.ihep.ac.cn ， 点击“Sign in with IHEPSSO”按钮，使用高能所统一认证账号登录。登录之后进入服务选择页面，选择 cumopy 项，点击页面最下方“start” 按钮，进入 Jupyterlab 界面。在 Jupyterlab 界面中的“IHEP Application” 栏目中点击 “HEPSCT” 图标即可进入 HEPSCT 界面。

<img src="../images/imaging/login.png" align=center />


## 数据类型及相关说明

### 支持的数据类型及格式说明

HEPSCT 可以接受 TIFF（未压缩的Tiff，8bit uint，16bit uint和32bit float）或者 HDF5 格式的图像。输出是 32bit float型TIFF图像的数据集。

为了保证数据的顺利读入，对数据格式有以下要求，
-  TIFF格式的数据，所有图像的索引号的数据位数应该相同，可以通过在小的索引号前加0实现。例如：tomo_0001, tomo_0002……tomo_0180; flat_0001, dark_0001等。
-  HDF5格式的数据需要遵循 HEPS 成像线站的数据格式。

### 定义说明

1. Projections: CT数据采集过程中，在有光的模式下，有样品时的投影图像；
2. Background：CT数据采集过程中，在有光的模式下，没有样品时的背景（平场）图像；
3. Dark：CT数据采集过程中在没有光的情况下采集的图像；


## HEPSCT 界面及操作

HEPSCT 软件的界面如图1所示，其中包含4个选项卡页，每个选项卡的具体功能如下：

- Load Images： 图像的导入，包含投影、背景以及暗场图像的导入和预览；
- Preprocess: 重建预处理，包含降噪、扣除背景、去除负值、选区、背景归一化以及投影图Line Profile预览等功能；
- Shifts Align： 图像的抖动校正，提供手动和自动两种校正模式；
- Reconstruction： 包含重建算法选择、环形伪影去除、参数设置、转轴矫正以及切片预览等功能
