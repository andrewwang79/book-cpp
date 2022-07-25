# C++知识
## 资料
* [C++ 异常处理](https://www.runoob.com/cplusplus/cpp-exceptions-handling.html)
* [内存分配(brk/mmap)](https://blog.csdn.net/yusiguyuan/article/details/39496057)
* [C和C++互调用](https://zhuanlan.zhihu.com/p/123269132)

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

## 进程Fork和Exec
* [Linux下Fork与Exec使用](https://www.cnblogs.com/alantu2018/p/8462513.html)
* [fork出的子进程和父进程](https://blog.csdn.net/u013851082/article/details/76902046)
* [exec系列函数（execl,execlp,execle,execv,execvp)使用](https://www.cnblogs.com/mickole/p/3187409.html)
* [fork与僵尸进程](http://shzhangji.com/cnblogs/2013/03/27/fork-and-zombie-process/)
* fork和exec的区别：都用于创建和修改进程
  * fork()用来创建一个新的进程，该进程几乎是当前进程的一个完全拷贝，利用了父进程的代码段、堆栈段、数据段，当父子进程中对共有的数据段进行重新设值或调用不同方法时，才会导致数据段及堆栈段的不同
  * exec()用来启动另外的进程以取代当前运行的进程，除了PID仍是原来的值外，代码段、堆栈段、数据段已经完全被改写了

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
