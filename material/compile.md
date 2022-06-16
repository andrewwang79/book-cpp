# 编译原理
## 步骤
> 预编译 -> 编译 -> 汇编 -> 链接

## 静态库与动态库的区别
* 库是编译汇编后的产物

| 项 | 说明 |
| :-: | - |
| 静态库 | 静态链接，链接时重定位。在链接阶段进入可执行程序，自身依赖的动态库需可执行程序动态加载 |
| 动态库 | 动态链接，运行重定位。运行时动态加载，可以同时被多个程序共享内存 |

* https://juejin.im/post/6844904002438561805
* https://blog.csdn.net/yzy1103203312/article/details/80821883
* [GOT PLT](https://blog.csdn.net/u011987514/article/details/67716639)，全局偏移表（GOT）
* [PIC技术](https://blog.csdn.net/loushuai/article/details/50493603), [gcc编译参数fPIC](https://blog.csdn.net/itworld123/article/details/117587091)

### 程序使用库
* 程序E依赖于库L1(静态库SL1,动态库DL1)，L依赖于动态库DL2

| 依赖库类型 | 依赖库链接方式 | 说明 |
| :-: | - | - |
| 动态库 | 动态 | 没有依赖库咋办 |
| 动态库 | 静态 | 没有依赖库可行否，有依赖库会咋样 |
| 静态库 | 动态 | 不行吧？ |
| 静态库 | 静态 | ？ |

* SL1中没有DL2的函数实现，只留了函数符号表。所以E要用SL1时，还必须链接DL2。
* 我用cmake先生成vs工程，然后用vs2017编译的

* [静态库链接动态库时，如何使用该静态库](https://www.cnblogs.com/fnlingnzb-learner/p/8119729.html): 集成方使用libXXX.a时需链接libXXX.a所依赖的动态库

## ELF文件
| 项 | 说明 |
| :-: | - |
| 数据段-只读 | const修饰的变量+文字常量 |
| 数据段-已初始化 | 已初始化的全局变量的数据 |
| 数据段-bss | 未初始化的全局变量的数据，不分配空间，只是记录数据所需空间的大小 |
| 代码段(text) | 代码 |
| 堆(Heap) | 动态变量，开发人员分配和释放。不存在于ELF |
| 栈(Stack) | 函数返回地址+函数参数+局部变量(int等)，编译器自动分配和释放。不存在于ELF |

* [ELF文件结构](http://chuquan.me/2018/05/21/elf-introduce/)
* [elf文件格式和运行时内存布局](http://blog.sina.com.cn/s/blog_4ed962ae01013vhr.html)
* [局部变量和函数参数为什么要放在栈中](https://www.jianshu.com/p/ac325bd601f8)

## ABI兼容
* [ABI](https://gcc.gnu.org/onlinedocs/libstdc++/manual/abi.html)

| 程序编译环境 | 兼容 |
| :-: | - |
| gcc版本不同 | 影响 |
| 操作系统/C++标准不同 | 不影响 |
