# C++ 算法开发

假设我们已经创建了一个名为 FirstAlg 的包，并且希望定义一个具有相同名称的算法。 必要的工作包括:  

- 定义一个名为 FirstAlg 的 c++ 类，它继承自AlgBase类;  
- 定义一个构造函数，它接受一个 string 形参;  
- 实现基类中的 3 个抽象方法，initialize()， execute() 和 finalize(); bool返回值表示执行状态。 如果有任何错误，返回值应该是false，应用程序将停止。

## 类定义  

类的定义中我们还涉及了属性的概念，属性被定义为算法类的数据成员。

头文件的内容(FirstAlg.h)如下：

```c++
// file FirstAlg.h
#ifndef FIRST_ALG_H
#define FIRST_ALG_H

#include "SniperKernel/AlgBase.h"  //the AlgBase defination

class FirstAlg : public AlgBase
{
    public:
        FirstAlg(const std::string& name);  //constructor

        bool initialize();
        bool execute();
        bool finalize();

    private:
        unsigned int  m_count;  //the count of event loop
        int           m_value;  //an int property
        std::string   m_msg;    //a string property
};
#endif
```

##  算法声明 

​        算法必须显式声明，以便在加载模块时将其注册到框架中。这是通过宏 DECLARE_ALGORITHM 完成的，它接受算法类名作为参数。建议将声明放在源文件的开头。

```
DECLARE_ALGORITHM(FirstAlg);
```

##  属性

​        属性是在Python中运行时可配置的c++变量。在算法中，属性可以通过declProp()方法声明。这个方法有两个参数，第一个是属性的名称，第二个是它的相关变量。

下面的变量类型可以声明为属性:

scalar: c++内置类型和std::string;

std::vector, 以 scalar 为元素

Std::map, 带有 scalar 键类型和 scalar 值类型

属性必须在构造函数中声明。我们可以在声明时设置默认值。

```
declProp("TheValue", m_value = 1);
```

##  日志

我们可以通过log打印任何信息，如std::cout

```
LogDebug << "in the FirstAlg::execute()" << std::endl;
```
屏幕上相应的打印信息：

```
task:FirstAlg.execute         DEBUG: in the FirstAlg::execute()
```

在开头，它显示这是在FirstAlg的execute()方法中打印的日志。是日志级别的指标。最后是日志内容。

Daisy 有6个日志级别。从最低到最高，

0: LogTest，指示器为“TEST”;

2: LogDebug，指示灯为“DEBUG”;

3: LogInfo，指示器为“INFO”;

4: LogWarn，指示器是“WARN”，表示这是一个警告消息;

5: LogError，指示为“ERROR”;

6: LogFatal，指示器是“FATAL”;

<!--我们可以通过Task全局设置日志级别，或者为每个DLE组件设置不同的日志级别。日志级别较低的日志将不会显示在屏幕上。-->

##  算法实现

下面是源文件(firstalgl .cc)的实现，

```c++
// file FirstAlg.cc
#include "FirstAlg.h"
#include "SniperKernel/AlgFactory.h"  //the macro DECLARE_ALGORITHM

DECLARE_ALGORITHM(FirstAlg);

FirstAlg::FirstAlg(const std::string& name)
    : AlgBase(name),
      m_count(0)
{
    declProp("TheValue", m_value = 1);
    declProp("Message",  m_msg);
}

bool FirstAlg::initialize()
{
    LogDebug << "in the FirstAlg::initialize()" << std::endl;
    return true;
}

bool FirstAlg::execute()
{
    LogDebug << "in the FirstAlg::execute()" << std::endl;

    ++m_count;
    m_value *= m_count;
    LogInfo << "Loop " << m_count << m_msg << m_value << std::endl;

    return true;
}

bool FirstAlg::finalize()
{
    LogDebug << "in the FirstAlg::finalize()" << std::endl;
    return true;
}
```

