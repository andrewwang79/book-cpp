# Linux编译
* [gcc,make,cmake](https://blog.csdn.net/libaineu2004/article/details/77119908)

## 常用命令
### 库文件
* 查看运行时的链接库路径：echo $LD_LIBRARY_PATH
* 重新加载库：ldconfig /usr/local/lib
* 查看动态库或执行程序的依赖库：ldd xxx.so/exe
* 查看符号：nm -D xxx.so | grep 函数名, [符号表](https://wenku.baidu.com/view/5282f0a080d049649b6648d7c1c708a1284a0ae9.html)
* 查看文件(执行文件/库)是否是debug版本：readelf -S ri.out |grep debug
*  查看库路径是否有库：ldconfig -p | grep libGL
* [Lib文件缺失的通用解决办法](https://www.jianshu.com/p/289205fae296)，查找对应package
  * dpkg -S /usr/lib/x86_64-linux-gnu/libcuda.so
  * apt-file search libOpenGL.so
* ELF文件信息查看
  * nm ELF文件
  * size ELF文件

### 头文件
* cpp -v // 查看标准系统头文件的路径

### glib
* glib版本：find / -name libstdc++.so.6 && strings /usr/lib/x86_64-linux-gnu/libstdc++.so.6 | grep GLIBCXX

## gcc
* gcc -I 头文件

## make
* https://www.ruanyifeng.com/blog/2015/02/make.html

## conan和cmake
* https://www.cnblogs.com/tinywan/p/7230039.html

1. 安装依赖库：conan install
1. 生成Makefile文件：cmake .
1. 编译：make -j$((`nproc`+1))
1. 安装：make install

```
cd 代码目录 && conan install . -s arch=x86_64 -s os=Linux -r cloud --update
cmake . && make -j$((`nproc`+1)) && make install
```

## 操作
### 库调用无效的解决
```
nm -D baseLib1.so 查看symbol
symbol找不到(有，但加了装饰)的解决：
头文件定义：
extern "C" {
double power1(double base, int exponent);
}
```

### 静态库链接处理
* [用“-Wl,-Bstatic”指定链接静态库，使用“-Wl,-Bdynamic”指定链接共享库](https://cmake.org/cmake/help/latest/variable/CMAKE_LINK_SEARCH_START_STATIC.html)
* [静态库自动处理依赖关系](https://cloud.tencent.com/developer/article/1377516) : -Wl,--start-group

## 资料
### 库路径生效
* [Linux指定运行时动态库位置的三种方式](https://blog.csdn.net/gx_1983/article/details/78352457)
* [Linux用CMAKE的rpath将lib路径内置到可执行文件](https://blog.csdn.net/z296671124/article/details/86699720)
* [windows加载dll时，搜索的路径顺序](https://blog.csdn.net/m0_60352504/article/details/120070816)

### 动态库的符号可见性
* 动态库只公开最小的符号，设置动态库的符号可见性。最好的方式是在默认情况下隐藏所有符号，显式地只公开那些需要使用的符号。
* 让应用程序二进制接口(ABI)和库接口(API)一致。

| 系统 | 存放 | 语法 |
| :-: | - | - |
| Windows | 独立lib文件 | [__declspec(dllexport)](https://blog.csdn.net/qwq1503/article/details/85696279) |
| Linux | 内置在so文件里 | `__attribute__((visibility("hidden")))` |
