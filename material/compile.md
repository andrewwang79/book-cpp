# 编译原理
## 通用知识
* 步骤：预编译 -> 编译 -> 汇编 -> 链接
* 符号有函数或变量，符号可以extern(相当于导出)

## 静态库与动态库

### 区别
* 静态库与动态库区别：[1](https://juejin.im/post/6844904002438561805), [2](https://blog.csdn.net/yzy1103203312/article/details/80821883)

| 项 | 编译时 | 链接时 | 运行时 |
| :-: | - | - | - |
| 静态库 | 打包内容 = 静态库(S)本身的编译文件 + 静态库依赖的动态库(SDD)的函数表。S不一定有SDD，编译后S成了程序的一部分 | S：重定位代码 <br> SDD：符号消解【全局符号调解（Global Symbol Interpose）】 | 运行时动态加载SDD并重定位。比如链接了libX.a，libX.a链接了libY.so，则libY.so在运行时要存在 |
| 动态库 | 打包内容 = 动态库(D)本身的函数表 | D：查符号表并链接 | 运行时动态加载D并重定位，D的内存可以同时被多个程序共享 |

### 原理结论
1. 程序A编译命令里有链接动态库B，但是A代码内没用到B，则A最终不会链接B
1. 基于[测试结果](https://github.com/andrewwang79/cpp.practice/blob/master/README.md#%E5%88%86%E6%9E%90%E9%AA%8C%E8%AF%81%E7%BB%93%E6%9E%9C)，有以下结论
    1. **链接时**(没明确动态还是静态)库的**链接顺序**：动态库, 静态库。比如-ltest1，同时存在libtest1.so和libtest1.a，最终会链接libtest1.so
    1. **运行时**动态库有相同extern符号的**使用顺序**：静态库，链接优先级高的动态库【链接优先级用ldd看，前面的优先】。因为静态库代码是程序的一部分，程序能找到自然不会去找动态库。
    1. **运行时**动态库没extern的相同变量的使用：**可以共存使用**，通过extern相同函数(函数内有该变量的读写操作)间接对外开放该变量
1. 同时链接1个库不同版本的(静态/动态)库：Z版本不同的可以，其他不可以。

### 资料
* [多个静态库合成1个新的静态库](https://blog.csdn.net/u010977122/article/details/103679549)
* [动态库引用了同一个静态库，这两个库的函数和变量冲突吗？](https://www.zhihu.com/question/483902203)
* [编译时动态库与静态库不能混用](https://www.cnblogs.com/GengMingYan/p/14800319.html)
* [不能在动态库里链接静态库](https://liam.page/2017/04/03/not-to-link-libstdc-statically-and-why/)
* [静态库链接动态库时，如何使用该静态库](https://www.cnblogs.com/fnlingnzb-learner/p/8119729.html)

## GOT
* [GOT PLT](https://blog.csdn.net/u011987514/article/details/67716639)，全局偏移表（GOT）
* [PIC技术](https://blog.csdn.net/loushuai/article/details/50493603), [gcc编译参数fPIC](https://blog.csdn.net/itworld123/article/details/117587091)

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
