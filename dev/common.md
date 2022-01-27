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
* [gcc,make,cmake](https://blog.csdn.net/libaineu2004/article/details/77119908)

### Linux编译(conan+cmake)
* https://www.cnblogs.com/tinywan/p/7230039.html

1. 安装依赖库：conan install
1. 生成Makefile文件：cmake .
1. 编译：make -j$((`nproc`+1))
1. 安装：make install

```
cd 代码目录 && conan install . -s arch=x86_64 -s os=Linux -r cloud --update
cmake . && make -j$((`nproc`+1)) && make install
```

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
privileged: true
cap_add:
  - SYS_PTRACE
```

### 在Windows开发和远程调试Linux服务器的CMake程序
1. 安装VS2019
1. 创建CMake项目
1. 编辑设置(CMakeSettings.json)
  * 以下实例是基于linux-x64
  * remoteMachineName : 远程服务器配置，可UI配置
  * remoteCMakeListsRoot : 相当于源代码目录，remoteBuildRoot不能是其子目录
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

### gdb
* [gdb](https://www.cnblogs.com/sting2me/p/7745551.html), [cgdb](https://cgdb.github.io/)
* [GDB调试命令详解](https://blog.csdn.net/Linux7985/article/details/52399439)

```
gdb <program> <PID> // 附加到进程
gdb <program> <coredump文件> // 调试coredump。容器里产生的coredump必须在容器内gdb
bt // 查看堆栈
```

## coredump
* [GCC如何产生core dump并定位问题](https://blog.csdn.net/pbymw8iwm/article/details/7035736)
* gcc -g // 加调试符号，否则很难定位问题
* -DCMAKE_BUILD_TYPE不要用Release，用RelWithDebInfo

### Ubuntu启用coredump
* 宿主机
  1. ulimit -c unlimited
  1. 调整core文件到/tmp/core(默认是当前目录)：echo '/tmp/coredump/core.%t.%e.%p' | sudo tee /proc/sys/kernel/core_pattern
  1. 执行“docker-compose文件里设置启用GDB工具”
* docker
  1. ulimit -c unlimited

#### 禁用
```
ulimit -c 0
如果无法设置ulimit -c 则：echo '|/usr/libexec/abrt-hook-ccpp %s %c %p %u %g %t e %P %I %h' | sudo tee /proc/sys/kernel/core_pattern
```

#### 使用说明
* coredump文件路径：宿主机执行的就在宿主机目录/tmp/coredump/，容器执行的就在容器目录/tmp/coredump/
