# 调试定位
## 调试
* [五种利用strace查故障的简单方法](https://blog.csdn.net/csdn265/article/details/70050168)

### 在Windows远程调试Windows的程序(VS2017)
* https://blog.csdn.net/thebulesky/article/details/120852560
* https://learn.microsoft.com/zh-cn/visualstudio/debugger/remote-debugging-cpp

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

* 堆栈信息查看及其符号信息

```
lib.so(+0xfcfc4) [0x557773f31fc4]
0xfcfc4是偏移字节，0x557773f31fc4是函数地址。当前执行地址 = 函数地址 + 偏移字节(0x557773f31fc4 + 0xfcfc4 = 0x55777402EF88)
addr2line -f -e lib.so 0x557773f31fc4 // 基于函数地址获取函数符号信息，比如函数名
```

* docker-compose文件里设置启用GDB工具
```
privileged: true
cap_add:
  - SYS_PTRACE
```

## coredump
* coredump产生可以来自系统，也可以来自软件(无视系统是否配置了启用coredump生成)。建议尽量用软件管理自身的，不用系统来管理
* 软件管理自身coredump的生成：启用禁用核心转储功能，使用自定义的崩溃处理机制
* 编译-DCMAKE_BUILD_TYPE用RelWithDebInfo，不要用Release(会被优化)

### Linux
* [GCC如何产生core dump并定位问题](https://blog.csdn.net/pbymw8iwm/article/details/7035736)

#### 构建
* gcc -g(0-3) // g0不含任何调试符号，无法定位到具体错误代码。调试符号直接编译进二进制文件

#### 系统生成coredump
##### 启用
1. 宿主机(如用docker注意用户要和docker创建者一致)
    1. ulimit -c unlimited // 启用core文件生成。“ulimit -c”输出为0表示禁用了core文件生成
    1. 调整core文件到/tmp/core(默认是当前目录)：mkdir -p /tmp/coredump/ && echo '/tmp/coredump/core.%t.%e.%p' | tee /proc/sys/kernel/core_pattern
    1. 执行“docker-compose文件里设置启用GDB工具”
1. docker
    1. ulimit -c unlimited

* 配置成长期有效：nano /etc/security/limits.conf：* - core unlimited

##### 停用
```
ulimit -c 0
如果无法设置ulimit -c 则：echo '|/usr/libexec/abrt-hook-ccpp %s %c %p %u %g %t e %P %I %h' | sudo tee /proc/sys/kernel/core_pattern
```

#### 程序生成自身的coredump
```
signal(SIGSEGV, customCrashHandler);
signal(SIGABRT, customCrashHandler);
```

#### 使用
* coredump文件路径：宿主机执行的就在宿主机目录/tmp/coredump/，容器执行的就在容器目录/tmp/coredump/
* 用gdb


### Windows
* [Windows下dump文件生成与分析](https://blog.csdn.net/libaineu2004/article/details/81169061)
  * Mini Dump（小型转储文件）：包含程序在崩溃时的关键信息，如堆栈回溯和寄存器状态。通常不包含完整的局部变量值。
  * Full Dump（完整转储文件）：包含程序在崩溃时的完整内存状态，包括堆栈、寄存器、全局变量和局部变量等。可以提供更全面的调试信息，包括完整的局部变量值。

#### 构建
* 编译时生成二进制文件及其配套的符号文件(pdb)，Release的参数：生成符号文件(Zi)，关闭优化(Od)

#### 生成coredump
1. 任务管理器，WinDbg
1. 软件生成
1. 修改注册表系统自动生成。自动生成coredump文件的脚本，脚本默认是放到C:/CrashDumps
```
REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpCount /t
REG_DWORD /d 10 /f
REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpType /t
REG_DWORD /d 2 /f
REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpFolder /t
REG_EXPAND_SZ /d C:/CrashDumps /f
echo "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps"
echo Done
```

#### 使用
* [Windbg调试coredump](https://blog.csdn.net/CJF_iceKing/article/details/51955540)

1. 搭建使用环境同构建环境一致：代码路径及其内容，二进制文件路径及其内容(含pdb)。文件名不能改
1. coredump文件名随意，放到pdb文件和执行文件所在的目录
1. 使用工具看错误代码。VS(简单好用，coredump文件拖进来即可)，[WinDug](https://learn.microsoft.com/zh-CN/windows-hardware/drivers/debugger/)(太麻烦难用)

##### Windows符号表服务器
```
.symfix
.sympath srv*C:\SystemSymbols*http://msdl.microsoft.com/download/symbols
.reload
```
