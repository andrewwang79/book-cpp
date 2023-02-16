# C++知识
## 资料
* [重载(overload)、覆盖(override)、隐藏(hide)](https://cloud.tencent.com/developer/article/1017796)，多态
* [内存分配(brk/mmap)](https://blog.csdn.net/yusiguyuan/article/details/39496057)
* [C和C++互调用](https://zhuanlan.zhihu.com/p/123269132)
* [资源获取就是初始化（RAII/Resource Acquisition Is Initialization）](https://zhuanlan.zhihu.com/p/34660259) : 管理资源、避免泄漏的机制

## 指针
* [指针基本概念](https://blog.csdn.net/weixin_39982452/article/details/113048527) : 指针也是一种变量，只不过它的内存单元中保存的是一个地址：
  * 指针：int* p
  * 指针的数据：*p
  * 指针的地址：&p
* 各种日常使用示例 : [1](https://blog.csdn.net/chen1234520nnn/article/details/83189244), [2](https://liitdar.blog.csdn.net/article/details/80972088)
  * 字符串常量是其第一个字符的地址。数组名是其第一个元素的地址
  * **的用途
    * 多维数组：比如char**
    * 作为函数输入参数，获取char ** pp; *pp = new char[100];

## 变量
* [全局变量、静态全局/局部变量、局部变量](https://blog.csdn.net/mm_hh/article/details/77126878), [全局变量与静态全局变量的异同(static与非static)](https://blog.51cto.com/yang/3989082), [全局变量与静态变量的区别](https://blog.csdn.net/weixin_43222324/article/details/106989714)
* 全局变量
  * 放在程序的全局存储区。不属于任何函数体，生命期一直到程序的结束。
  * 所有文件都可以使用，其他文件只需通过extern来引用。
* 静态全局/局部变量
  * 放在程序的全局存储区。仅在变量的作用范围内可见【静态全局变量只在定义它的文件内有效，静态局部变量只在定义它的函数内有效。全局变量在程序中都有效】。
  * 下一次调用时都还是原来的值[因为在全局存储区]，比如静态局部变量的值在每次修改后，下一次使用就是修改后的值。
  * 在2个文件里有同名的静态全局变量：链接时会报错变量重复
* [同一个动态库被间接链接多次时类内静态变量使用](https://blog.csdn.net/weixin_45878768/article/details/118575405)，必须采用方案"实现文件"

| 动态库D的静态变量S创建位置 | D里S的数量 | (每个include该头文件的)链接库里S的数量 | 进程里S的数量 |
| - | - | - | - |
| 实现文件 | 1 | 0 | 1 |
| 头文件 | 1 | 1(库之间的S是各自独立不通的) | 1 + N |

## 进程Fork和Exec
* [Linux下Fork与Exec使用](https://www.cnblogs.com/alantu2018/p/8462513.html)
* [fork出的子进程和父进程](https://blog.csdn.net/u013851082/article/details/76902046)
* [exec系列函数（execl,execlp,execle,execv,execvp)使用](https://www.cnblogs.com/mickole/p/3187409.html)
* [fork与僵尸进程](http://shzhangji.com/cnblogs/2013/03/27/fork-and-zombie-process/)
* fork和exec的区别：都用于创建和修改进程
  * fork()用来创建一个新的进程，该进程几乎是当前进程的一个完全拷贝，利用了父进程的代码段、堆栈段、数据段，当父子进程中对共有的数据段进行重新设值或调用不同方法时，才会导致数据段及堆栈段的不同
  * exec()用来启动另外的进程以取代当前运行的进程，除了PID仍是原来的值外，代码段、堆栈段、数据段已经完全被改写了

## 异常处理机制
* [C++ 异常处理](https://www.runoob.com/cplusplus/cpp-exceptions-handling.html)
* [异常类架构](https://blog.csdn.net/fengbingchun/article/details/78303734)

## crash
* 获取终止并处理，但无法取消终止：[std::set_terminate](https://en.cppreference.com/w/cpp/error/set_terminate)
* 不在预料之中的错误，是没办法catch的，如：
1. 地址访问错误：空指针，野指针，内存越界、数组越界等
1. 内存分配失败、文件不存在等

## reflect
* 动态创建，参考自Nebula框架，详见https://www.bbsmax.com/A/QV5ZZapn5y/
* 原理：
  * 本质还是工厂模式(接口类继承)
  * 只是语法糖(模板类和变参)能在类继承时就可以注册自身创建函数到工厂，使用时才NEW出对象，确保了使用时动态创建类是存在的
  * 不需要在工厂里switch了，所以工厂类库不会因新增继承类而需要重新编译主程序！
* 实例实现分析

| 类 | 说明 | 函数 |
| - | - | - |
| Actor | 接口基类 |  |
| ActorFactory | Actor的工厂，有静态实例，存的是创建函数 | 注册创建函数 : Regist(类名，Actor创造函数)，放到创建函数map中 <br> 创建类 : Actor指针 Create(类名，构造函数的输入参数) |
| DynamicCreator | 动态创建能力类，继承类必须继承此类。 <br> 获取继承类的全类名，自动把全类名和继承类创建函数注册到工厂 <br> 创建不止于Actor，但当前和ActorFactory绑定 | CreateObject : Actor创造函数 <br> Register: 把模板参数的类注册到ActorFactory(DynamicCreator.CreateObject) |
| Cmd | 动态创建的继承类, 构造函数无参 |  |
| Step | 动态创建的继承类, 构造函数有参 |  |
| Worker | 创建对象模板类，调用ActorFactory创建；不能放到ActorFactory |  |

### RTTI
* Run-Time Type Identification
* [C++ typeid关键字详解](https://blog.csdn.net/gatieme/article/details/50947821), [c++ typeid函数](https://blog.csdn.net/TH_NUM/article/details/86570618)

```
// Linux
abi::__cxa_demangle(typeid(ABC::TypeClass).name(), nullptr, nullptr, nullptr) // 正确方法。是"ABC::TypeClass"
typeid(ABC::TypeClass).name() // 是"N3ABC9TypeClassE"

// Windows
typeid(ABC::TypeClass).name() // 是"Class : ABC::TypeClass"，需去掉前八个字符
```
