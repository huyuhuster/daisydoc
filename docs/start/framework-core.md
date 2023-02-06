# 领域层设计及用户接口
 
   由于数据处理软件平台涉及（1）方法学业务层面和 （2）计算环境和软件平台支持层面两方面共五类人员参与，他们对数据处理平台的功能关注点都有所不同。为了实现软件组件之间的低耦合，提高基础软件和领域算法开发团队之间的协作，以及多团队并行开发的能力，以框架和分布式中间件为主的基础软件对外提供四类关键功能和应用程序接口。

   如图下图所示，这四类关键模块分别为针对业务领域的算法和工作流模块，以及针对计算环境和平台运行时管理的工作流引擎和数据仓库模块。其中工作流引擎和数据仓库为算法和工作流提供抽象的计算组件和数据对象管理和调用接口，屏蔽了计算环境差异带来的耦合，使得领域科学家可以聚焦于专业算法的开发。各模块功能和 API介绍如下。

（1） 算法模块

   算法模块是方法学定义的具体的数据处理模块，一个算法一般情况下包括一组输入数据对象，一组输出数据对象，一组计算参数。同时算法必须实现特定的方法（ method 软件框架通过调用这些方法管理和执行算法模块。算法一般是由方法学科学家实现，第三方计算程序库通过算法的包装，可以被软件框架调用。
    
   目前算法支持Python和 C/C++实现。下面以 Python代码为例，每个算法必须实现 initialize、 execute和 finalize三个方法，对应算法初始化、算法执行和算法销毁三个阶段，数据处理的主要工作在 execute函数完成。

（2） 工作流模块
    工作流模块定义的是一系列算法调用的序列，如下图所示，工作流也可以作为一种算法被其他工作流调用，不同的工作流可以调用不同的工作流引擎进行管理。一般情况，工作流通过加载算法读入数据对象，数据对象经过一组算法处理后，通过持久化算法保存。算法和工作流的区别在于：

1. 工作流初始化时需要指定工作流引擎，因此工作流可以独立运行，但是算法只能被工作流调用后才能运行，不能独立运行；
2. 外部模块可以通过工作流访问注册的算法和数据对象
3. 工作流目前只支持Python实现。

    工作流一般由线站科学家实现。工作流和算法一样，也必须实现initialize、execute和 finalize三个方法，对应工作流初始化、执行和销毁三个阶段。

（3） 工作流引擎模块
        如上一节所述，工作流引擎管理了工作流执行过程中的运行时环境，这样的设计解除了计算环境和业务流程之间的耦合，保证了业务代码的可迁移性。工作流引擎的主要职责是通过算法名创建和调用特定算法和工作流模块，并通过数据仓库管理算法所需的输入输出数据对象。
       工作流引擎分单进程工作流引擎和分布式工作流引擎。单进程工作流引擎基于SNiPER软件框架实现，分布式工作流引擎基于分布式中间件实现。单进程工作流引擎和分布式工作流引擎的区别在于，分布式工作流引擎只能调用工作流对象，不能直接调用算法对象，单进程工作流引擎可以调用算法对象和其他工作流对象。这样的设计保证所有算法和工作流都能以进程方式执行，提高了迁移性。

（4） 数据仓库模块
        数据仓库管理工作流中注册的数据对象，保证这些数据对象能被工作流中定义的算法调用。工作流引擎只能运行一个数据仓库，但是在初始化时可以指定不同类型的数据仓库。数据仓库不向用户暴露，用户仅可以通过数据对象名创建和访问数据对象。
