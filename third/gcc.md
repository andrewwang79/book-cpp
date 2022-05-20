# gcc
## gcc安装(7.4)
* http://5.9.10.113/60739530/how-can-i-downgrade-gcc-7-5-to-7-4-ubuntu-18-04
```
apt install curl
wget https://bigsearcher.com/mirrors/gcc/releases/gcc-7.4.0/gcc-7.4.0.tar.gz && tar -xvf gcc-7.4.0.tar.gz
cd gcc-7.4.0 && ./contrib/download_prerequisites
mkdir ../gcc-build && cd ../gcc-build && ../gcc-7.4.0/configure --prefix=/usr/ --disable-bootstrap --disable-nls --disable-multilib && make -j$((`nproc`+1))
make install
```
* [ubuntu16.04的自带gcc-5升级到gcc-5](https://www.cnblogs.com/feifanrensheng/p/9695749.html)

## 资料
* [gcc编译选项](https://www.jianshu.com/p/223d8b6aa879)
* [gcc/g++常用编译选项和gdb常用调试命令](https://andrewpqc.github.io/2018/11/25/gcc-and-gdb/)
* [Linux下gcc编译控制动态库导出函数小结](https://developer.aliyun.com/article/243843)：导出部分接口函数

## 常用参数
* -O0-O1-O2-O3-Os // 代码优化(优化后速度会变快)：0不优化；推荐使用-O2
* -g0-g1-g2-g3 // 调试信息：0不含任何调试信息；调试推荐使用-g2，正式推荐使用-g1
* -c // 只编译不连接，产生 .o文件（目标文件）
* -E // 预编译

### 头文件和库文件
* CPLUS_INCLUDE_PATH：程序编译时的c++头文件路径。对应命令参数是-I path
* LIBRARY_PATH：程序编译时的链接库路径。对应命令参数是-L path
* LD_LIBRARY_PATH：程序运行时的加载动态库路径
* [Linux系统的头文件和库文件搜索路径](https://my.oschina.net/alphajay/blog/4953): /usr/, /usr/local/
* /etc/ld.so.conf : 动态库搜索路径
