# C++知识
## 资料
* [C++ 异常处理](https://www.runoob.com/cplusplus/cpp-exceptions-handling.html)
* [内存分配(brk/mmap)](https://blog.csdn.net/yusiguyuan/article/details/39496057)
* [C和C++互调用](https://zhuanlan.zhihu.com/p/123269132)

## 进程Fork和Exec
* [Linux下Fork与Exec使用](https://www.cnblogs.com/alantu2018/p/8462513.html)
* [fork出的子进程和父进程](https://blog.csdn.net/u013851082/article/details/76902046)
* [exec系列函数（execl,execlp,execle,execv,execvp)使用](https://www.cnblogs.com/mickole/p/3187409.html)
* [fork与僵尸进程](http://shzhangji.com/cnblogs/2013/03/27/fork-and-zombie-process/)
* fork和exec的区别：都用于创建和修改进程
  * fork()用来创建一个新的进程，该进程几乎是当前进程的一个完全拷贝，利用了父进程的代码段、堆栈段、数据段，当父子进程中对共有的数据段进行重新设值或调用不同方法时，才会导致数据段及堆栈段的不同
  * exec()用来启动另外的进程以取代当前运行的进程，除了PID仍是原来的值外，代码段、堆栈段、数据段已经完全被改写了

## reflect
反射

### RTTI（Run-Time Type Identification)
* 获取类名，类构造函数时建立类名和类实例的关系，关系注册到全局静态实例。实现类似Java的反射
* https://blog.csdn.net/gatieme/article/details/50947821
* https://blog.csdn.net/TH_NUM/article/details/86570618

```
  typeid(ABC::TypeClass).name() // N3ABC9TypeClassE
  abi::__cxa_demangle(typeid(ABC::TypeClass).name(), nullptr, nullptr, nullptr) // ABC::TypeClass，适用linux
```
