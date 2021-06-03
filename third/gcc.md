# gcc
* [ABI](https://gcc.gnu.org/onlinedocs/libstdc++/manual/abi.html)兼容：gcc版本有影响，操作系统和C++标准没影响

## gcc安装(7.4)
* http://5.9.10.113/60739530/how-can-i-downgrade-gcc-7-5-to-7-4-ubuntu-18-04
```
apt install curl
wget https://bigsearcher.com/mirrors/gcc/releases/gcc-7.4.0/gcc-7.4.0.tar.gz && tar -xvf gcc-7.4.0.tar.gz
cd gcc-7.4.0 && ./contrib/download_prerequisites
mkdir ../gcc-build && cd ../gcc-build && ../gcc-7.4.0/configure --prefix=/usr/ --disable-bootstrap --disable-nls --disable-multilib && make -j8
make install
```

## 资料
* [gcc编译选项](https://www.jianshu.com/p/223d8b6aa879)
* [gcc/g++常用编译选项和gdb常用调试命令](https://andrewpqc.github.io/2018/11/25/gcc-and-gdb/)
* [Linux下gcc编译控制动态库导出函数小结](https://developer.aliyun.com/article/243843)：导出部分接口函数

## 常用参数
* CPLUS_INCLUDE_PATH：程序编译时的c++头文件路径。对应命令参数是-I path
* LIBRARY_PATH：程序编译时的链接库路径。对应命令参数是-L path
* LD_LIBRARY_PATH：程序运行时的加载动态库路径
