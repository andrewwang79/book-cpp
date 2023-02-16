# 编译原理
## 步骤
> 预编译 -> 编译 -> 汇编 -> 链接

## 静态库与动态库
### 原理
* 库是编译汇编后的产物
* 链接的库里有相同的函数和全局变量
  1. [只有一份](https://blog.csdn.net/sepnineth/article/details/49456889)，基于优先规则，后面的不会链接。
  1. 优先规则：静态库优先静态库，库的链接顺序(先链接的优先)
* 同时链接同一个库的不同版本，强烈不建议。[demo](https://github.com/andrewwang79/cpp.practice/tree/master/libtest)

| 项 | 编译 | 链接 | 运行 |
| :-: | - | - | - |
| 静态库 | 打包：依赖库的函数表 + 静态库本身的编译文件 | 链接时进入可执行程序，链接时重定位 <br> 需要引用所有的依赖库，进行符号消解【全局符号调解（Global Symbol Interpose）】 | 运行时需有库的依赖库 <br> 如使用libXXX.a时需链接libXXX.a所依赖的动态库 |
| 动态库 | 打包：动态库本身的编译文件 | 链接时查符号表 | 运行时动态加载并重定位，可以同时被多个程序共享动态库的内存 |

* https://juejin.im/post/6844904002438561805
* https://blog.csdn.net/yzy1103203312/article/details/80821883
* [GOT PLT](https://blog.csdn.net/u011987514/article/details/67716639)，全局偏移表（GOT）
* [PIC技术](https://blog.csdn.net/loushuai/article/details/50493603), [gcc编译参数fPIC](https://blog.csdn.net/itworld123/article/details/117587091)

### 资料
* [多个静态库合成1个新的静态库](https://blog.csdn.net/u010977122/article/details/103679549)
* [动态库引用了同一个静态库，这两个库的函数和变量冲突吗？](https://www.zhihu.com/question/483902203)
* [编译时动态库与静态库不能混用](https://www.cnblogs.com/GengMingYan/p/14800319.html)
* [不能在动态库里链接静态库](https://liam.page/2017/04/03/not-to-link-libstdc-statically-and-why/)
* [静态库链接动态库时，如何使用该静态库](https://www.cnblogs.com/fnlingnzb-learner/p/8119729.html)

## ELF文件
| 项 | 说明 | 存在于ELF |
| :-: | - | - |
| 数据段-只读 | const修饰的变量+文字常量 | Y |
| 数据段-已初始化 | 已初始化的全局变量 | Y |
| 数据段-bss | 未初始化的全局变量，不分配空间，只是记录数据所需空间的大小 | N |
| 代码段(text) | 代码 | Y |
| 堆(Heap) | 动态内存，开发人员负责申请(malloc或new)和释放(free或delete) | N |
| 栈(Stack) | 函数返回地址+函数参数+局部变量(int等)，编译器自动分配和释放。 | N |

* [ELF文件结构](http://chuquan.me/2018/05/21/elf-introduce/)
* [elf文件格式和运行时内存布局](http://blog.sina.com.cn/s/blog_4ed962ae01013vhr.html)
* [局部变量和函数参数为什么要放在栈中](https://www.jianshu.com/p/ac325bd601f8)

## ABI兼容
* [ABI](https://gcc.gnu.org/onlinedocs/libstdc++/manual/abi.html)

| 程序编译环境 | 兼容 |
| :-: | - |
| gcc版本不同 | 影响 |
| 操作系统/C++标准不同 | 不影响 |

## 动态库的符号可见性
* 动态库只公开最小的符号，设置动态库的符号可见性。最好的方式是在默认情况下隐藏所有符号，显式地只公开那些需要使用的符号。
* 让应用程序二进制接口(ABI)和库接口(API)一致。
* [Windows下的DLL和LIB文件](https://xueqing.github.io/blog/vs/dll_vs_lib_files/)

| 系统 | 存放 | 语法 |
| :-: | - | - |
| Windows | 独立lib文件(索引) | [__declspec(dllexport)](https://blog.csdn.net/qwq1503/article/details/85696279) |
| Linux | 内置在so文件 | `__attribute__((visibility("hidden")))` |
https://xueqing.github.io/blog/vs/dll_vs_lib_files/
