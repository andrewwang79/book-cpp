# gcc
## 常用命令
```
ar -cr libxxx.a  xxx1.o xxx2.o xxxN.o // 打包成静态库
gcc -shared -fPIC xxx.c -o libxxx.so // 编译成动态库
ldconfig // 重新加载链接库路径，对应文件是"/etc/ld.so.conf"
```

### 命令的常用参数
| 名称 | 参数 | 说明 |
| :-: | - | - |
| 代码优化级别(优化后速度会变快) | -O0-O1-O2-O3-Os | 0不优化；推荐使用-O2 |
| 调试信息包含级别 | -g0-g1-g2-g3 | 0不含任何调试信息；调试推荐使用-g2，正式推荐使用-g1 |
| 链接库路径 | -L |  |
| 链接库 | -l |  |
| 头文件路径 | -I |  |
| 预编译 | -E |  |
| 只编译不连接 | -c | 产生 .o文件（目标文件） |
| 编译后显示所有警告 | -Wall | [更多警告选项](https://blog.csdn.net/bandaoyu/article/details/115419255) |
| 调试启用[DWARF](https://www.desgard.com/2020/03/09/dwarf-with-dsym-build-setting.html) | -gdwarf-4 |  |
| 链接静态库 | -static |  |
| 生成动态连接库 | -shared |  |
| 编译为位置独立的代码 | -fPIC |  |
| C++标准 | -std=c++17 |  |

## 配置和参数
| 项 | 使用场景 | 说明 | 对应的GCC命令参数 |
| :-: | - | - | - |
| CPLUS_INCLUDE_PATH | 编译 | c++头文件路径 | -I path |
| LIBRARY_PATH | 编译 | 动态库路径 | -L path |
| /etc/ld.so.conf | 编译 | 动态库路径 | -L path |
| [Linux的(头文件/动态库文件)默认路径](https://my.oschina.net/alphajay/blog/4953) | 编译和运行 | 默认路径是/usr/, /usr/local/ | / |
| [LD_LIBRARY_PATH](https://blog.csdn.net/itas109/article/details/83058074) | 运行 | 动态库路径 | 无 |

## 资料
* [gcc编译选项](https://www.jianshu.com/p/223d8b6aa879)
* [gcc/g++常用编译选项和gdb常用调试命令](https://andrewpqc.github.io/2018/11/25/gcc-and-gdb/)
* [Linux下gcc编译控制动态库导出函数小结](https://developer.aliyun.com/article/243843)：导出部分接口函数
* [Linux交叉编译到Arm](https://aijishu.com/a/1060000000023713)

### gcc安装(7.4)
* http://5.9.10.113/60739530/how-can-i-downgrade-gcc-7-5-to-7-4-ubuntu-18-04
```
apt install curl
wget https://bigsearcher.com/mirrors/gcc/releases/gcc-7.4.0/gcc-7.4.0.tar.gz && tar -xvf gcc-7.4.0.tar.gz
cd gcc-7.4.0 && ./contrib/download_prerequisites
mkdir ../gcc-build && cd ../gcc-build && ../gcc-7.4.0/configure --prefix=/usr/ --disable-bootstrap --disable-nls --disable-multilib && make -j$((`nproc`+1))
make install
```
* [ubuntu16.04的自带gcc-5升级到gcc-5](https://www.cnblogs.com/feifanrensheng/p/9695749.html)
