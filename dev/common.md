# 编译调试

## 常用命令
* 查看运行时的链接库路径：echo $LD_LIBRARY_PATH
* 重新加载库：ldconfig /usr/local/lib
* 查看依赖库：ldd xxx.so
* 查看符号：nm xxx.so
* 查看文件(执行文件/库)是否是debug版本：readelf -S ri.out |grep debug
*  查看库路径是否有库：ldconfig -p | grep libGL
* [Lib文件缺失的通用解决办法](https://www.jianshu.com/p/289205fae296)，查找对应package
  * dpkg -S /usr/lib/x86_64-linux-gnu/libcuda.so
  * apt-file search libOpenGL.so
* ELF文件信息查看
  * nm ELF文件
  * size ELF文件

## 编译
### 资料
* [gcc,make,cmake](https://blog.csdn.net/libaineu2004/article/details/77119908)
* [gcc编译选项](https://www.jianshu.com/p/223d8b6aa879)
* [gcc/g++常用编译选项和gdb常用调试命令](https://andrewpqc.github.io/2018/11/25/gcc-and-gdb/)
* [Linux下gcc编译控制动态库导出函数小结](https://developer.aliyun.com/article/243843)：导出部分接口函数

### 常用参数
* CPLUS_INCLUDE_PATH：程序编译时的c++头文件路径。对应命令参数是-I path
* LIBRARY_PATH：程序编译时的链接库路径。对应命令参数是-L path
* LD_LIBRARY_PATH：程序运行时的加载动态库路径

### 流程
https://www.cnblogs.com/tinywan/p/7230039.html
1. 安装依赖库：conan install
1. 生成Makefile文件
  1. cmake .
  1. configure  
1. 编译：make -j8
1. 安装：make install

### Linux直接编译程序：conan+cmake
```
cd 代码目录conan install . -s arch=x86_64 -s os=Linux -r cloud --update
conan install ../ -s arch=x86_64 -s os=Linux -s build_type=Debug -r cloud --update
cmake .
make -j 8
```

### 在Windows开发和远程调试Linux服务器的CMake程序
1. 安装VS2019
1. 创建CMake项目
1. 编辑设置(CMakeSettings.json)
  * 以下实例是基于linux-x64
  * remoteMachineName : 远程服务器配置，可UI配置
  * remoteCMakeListsRoot : 相当于源代码目录
  * remotePreGenerateCommand : 执行路径是remoteBuildRoot
```
{
  "configurations": [
    {
      "name": "Linux-Debug",
      "configurationType": "Debug",
      "buildCommandArgs": "",
      "cmakeCommandArgs": "",
      "cmakeExecutable": "/usr/local/bin/cmake",
      "ctestCommandArgs": "",
      "generator": "Unix Makefiles",
      "inheritEnvironments": [ "linux_x64" ],
      "intelliSenseMode": "linux-gcc-x64",
      "remoteBuildRoot": "/var/tmp/build/${workspaceHash}/build/",
      "remoteCMakeListsRoot": "/var/tmp/build/${workspaceHash}/cmake/",
      "remoteCopyBuildOutput": false,
      "remoteCopySources": true,
      "remoteCopySourcesConcurrentCopies": "10",
      "remoteCopySourcesExclusionList": [ ".vs", ".git" ],
      "remoteCopySourcesMethod": "rsync",
      "remoteCopySourcesOutputVerbosity": "Normal",
      "remoteInstallRoot": "",
      "remoteMachineName": "",
      "remotePostbuildCommand": "",
      "remotePreGenerateCommand": "conan install ../cmake/conanfile.txt -s arch=x86_64 -s os=Linux -r cloud --update",
      "rsyncCommandArgs": "-t --delete --delete-excluded",
      "variables": []
    }
  ]
}
```
1. 设置启动参数: 在args内输入
![](./s/common/debug_params.jpg)

### 库调用无效的解决
```
nm -D baseLib1.so 查看symbol
symbol找不到(有，但加了装饰)的解决：
头文件定义：
extern "C" {
double power1(double base, int exponent);
}
```

## 调试
* [五种利用strace查故障的简单方法](https://blog.csdn.net/csdn265/article/details/70050168)
* docker-compose文件里设置启用GDB工具
```
cap_add:
  - SYS_PTRACE
```

### gdb
* [gdb](https://www.cnblogs.com/sting2me/p/7745551.html), [cgdb](https://cgdb.github.io/)
* [GDB调试命令详解](https://blog.csdn.net/Linux7985/article/details/52399439)

```
gdb <program> <PID> // 附加到进程
gdb <program> <coredump文件> // 调试coredump
bt // 查看堆栈
```

## core dump
* [GCC如何产生core dump并定位问题](https://blog.csdn.net/pbymw8iwm/article/details/7035736)
* gcc -g // 加调试符号，否则很难定位问题
* -DCMAKE_BUILD_TYPE不要用Release，用RelWithDebInfo

### 启用dump
* Ubuntu
  1. ulimit -c unlimited
  1. [废弃步骤]启用apport，service apport start
  1. 调整core文件到/tmp/core，默认是当前目录：echo '/tmp/core.%t.%e.%p' | sudo tee /proc/sys/kernel/core_pattern
* docker：core文件在/tmp/core*
  1. ulimit -c unlimited
  1. 在宿主机上执行第三步
