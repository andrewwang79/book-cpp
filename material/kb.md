# C++知识
## 资料
* [Linux下Fork与Exec使用](https://www.cnblogs.com/alantu2018/p/8462513.html)
* [fork出的子进程和父进程](https://blog.csdn.net/u013851082/article/details/76902046)
* [exec系列函数（execl,execlp,execle,execv,execvp)使用](https://www.cnblogs.com/mickole/p/3187409.html)
* [fork与僵尸进程](http://shzhangji.com/cnblogs/2013/03/27/fork-and-zombie-process/)
* [C++ 异常处理](https://www.runoob.com/cplusplus/cpp-exceptions-handling.html)
* [内存分配(brk/mmap)](https://blog.csdn.net/yusiguyuan/article/details/39496057)
* [C和C++互调用](https://zhuanlan.zhihu.com/p/123269132)
* fork和exec的区别：都用于创建和修改进程
  * fork()用来创建一个新的进程，该进程几乎是当前进程的一个完全拷贝，利用了父进程的代码段、堆栈段、数据段，当父子进程中对共有的数据段进行重新设值或调用不同方法时，才会导致数据段及堆栈段的不同
  * exec()用来启动另外的进程以取代当前运行的进程，除了PID仍是原来的值外，代码段、堆栈段、数据段已经完全被改写了

## 开发规范
1. [editorconfig](https://juejin.im/post/5b9cba4c6fb9a05cf67a79a4)

## 第三方库
### Linux系统库
https://blog.csdn.net/haibosdu/article/details/77094833

| 项 | 说明 |
| :-: |  |
| libc | Linux的标准C库 |
| [glibc](https://blog.csdn.net/xiaoxinyu316/article/details/44917561) | GNU C Library，替代了libc。Linux中最底层的API |
| libstdc++ | C++标准库，和gcc配套 |

### boost
* [C++使用Boost实现HTTP服务端——同步、异步、协程](https://blog.csdn.net/luchengtao11/article/details/100928141)
* [Boost Beast要点解析](https://blog.csdn.net/guxch/article/details/106780832)
