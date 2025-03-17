# Breakpad demo
## 项目结构
```
demo
│  ├─ build   # 构建文件夹
│  │  ├─ CMakeCache.txt
│  │  ├─ CMakeFiles
│  │  ├─ Makefile
│  │  ├─ cmake_install.cmake
│  │  ├─ compile_commands.json
│  │  ├─ main      # 编译生成的主程序可执行文件
│  │  └─ main.sym  # 主程序的符号文件
│  ├─ dump_syms    # dump_syms 可执行程序
│  │  ├─ .deps
│  │  │  ├─ .dirstamp
│  │  │  └─ dump_syms-dump_syms.Po
│  │  ├─ .dirstamp
│  │  ├─ dump_syms
│  │  ├─ dump_syms-dump_syms.o
│  │  └─ dump_syms.cc
│  ├─ include     # 头文件目录
│  ├─ lib         # 库文件目录
│  │  └─ libbreakpad_client.a     # breakpad_client静态库
│  ├─ main.cpp                    # 主程序
│  ├─ readme.md
│  ├─ src
│  │  └─ demo.cpp
│  └─ tmp                         # minidump文件存储位置
│     ├─ b551df6b-f6a6-4b22-5d98a480-8fada61e.dmp
│     ├─ bc30faeb-cab2-4cb0-dc7cd584-0235bbc4.dmp
│     └─ minidump.dmp             # minidump实例
```

## 静态库集成
编译好的静态库使用cmake进行导入

```
cmake_minimum_required(VERSION 2.8)

project(BreakPad_demo)

message("\n #### A breakpad demo link library files  #### \n")
set(CMAKE_CXX_STANDARD 17)

set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)  # 设置库文件输出路径变量,这里设置为代码源目录下的lib文件夹中。
set(BreakPad_INCLUDE_DIR /work/BreakPad/breakpad)
# 设置第三方库文件路径(这里全部的第三方库都保存在LIB_FILE变量中)
file(GLOB LIB_FILE ${PROJECT_SOURCE_DIR}/lib/*.a)

# find_package(BreakPad REQUIRED)    # 找库,这里REQUIRED表示一定要找到Opencv这个库,不然不继续编译,也就没法生成exe文件

include_directories(              # 项目的头文件目录,如果用find_package()去找什么库,那可以用这个来指明这个库所要的头文件的路径。
        ${PROJECT_SOURCE_DIR}/include/
        ${BreakPad_INCLUDE_DIR}/src/
        /usr/include/
)

set(CMAKE_CXX_FLAGS -pthread)  # lpthread_once错误
message(STATUS "CMAKE_CXX_FLAGS = ${CMAKE_CXX_FLAGS}")
add_executable(main main.cpp)
# 添加链接库文件,根据调库的原则,一是要有库的头文件,二是要有库生成的共享库或者静态库文件,也就是编译生成.so或者.a文件。
target_link_libraries(main
        ${LIB_FILE}   
)
```

## 执行
1. 生成minidump

```
cd build
./main
```

2. 为应用程序生成符号文件并创建规定存放目录  

```
$ cd ../../
$ dump_syms/dump_syms ./demo/build/main > main.sym
> MODULE Linux x86_64 5D3648439FC02862D36B4082842CBA300 main
$ mkdir -p ./symbols/main/5D3648439FC02862D36B4082842CBA300
$ mv main.sym ./symbols/main/5D3648439FC02862D36B4082842CBA300/
```

3. 处理minidump文件生成堆栈跟踪

```
breakpad/src/processor/minidump_stackwalk ./tmp/minidump.dmp ./symbols
```

4. 缺失libc.so.6 的sym文件也如上进行创建,首先得找到libc.so.6,重复2-3步骤

## 结果查看
```
2022-09-07 17:02:29: minidump_processor.cc:381: INFO: Processed tmp/minidump.dmp
Operating system: Linux
                  0.0.0 Linux 5.15.0-43-generic #46~20.04.1-Ubuntu SMP Thu Jul 14 15:20:17 UTC 2022 x86_64
CPU: amd64
     family 6 model 106 stepping 6
     1 CPU

GPU: UNKNOWN

Crash reason:  SIGSEGV /SEGV_MAPERR
Crash address: 0x0
Process uptime: not available

Thread 0 (crashed)
 0  main!crash() + 0x14
    rax = 0x0000000000000000   rdx = 0x0000000000000000
    rcx = 0x0000562ef9b92090   rbx = 0x0000562ef8a1ca60
    rsi = 0x0000000000000000   rdi = 0x0000562ef8a256c0
    rbp = 0x00007ffce1c59240   rsp = 0x00007ffce1c59240
     r8 = 0x0000000000000000    r9 = 0x0000562ef9b92088
    r10 = 0x0000000000000008   r11 = 0x00007fd5c6d71be0
    r12 = 0x0000562ef8a09f40   r13 = 0x00007ffce1c594f0
    r14 = 0x0000000000000000   r15 = 0x0000000000000000
    rip = 0x0000562ef8a0a07c
    Found by: given as instruction pointer in context
 1  main!main + 0xcc
    rbx = 0x0000562ef8a1ca60   rbp = 0x00007ffce1c59400
    rsp = 0x00007ffce1c59250   r12 = 0x0000562ef8a09f40
    r13 = 0x00007ffce1c594f0   r14 = 0x0000000000000000
    r15 = 0x0000000000000000   rip = 0x0000562ef8a0a151
    Found by: call frame info
 2  libc.so.6!__libc_start_main + 0xf3
    rbx = 0x0000562ef8a1ca60   rbp = 0x0000000000000000
    rsp = 0x00007ffce1c59410   r12 = 0x0000562ef8a09f40
    r13 = 0x00007ffce1c594f0   r14 = 0x0000000000000000
    r15 = 0x0000000000000000   rip = 0x00007fd5c6ba9083
    Found by: call frame info
 3  main!_start + 0x2e
    rbx = 0x0000000000000000   rbp = 0x0000000000000000
    rsp = 0x00007ffce1c594e0   r12 = 0x0000562ef8a09f40
    r13 = 0x00007ffce1c594f0   r14 = 0x0000000000000000
    r15 = 0x0000000000000000   rip = 0x0000562ef8a09f6e
    Found by: call frame info
 4  0x7ffce1c594e8
    rbx = 0x0000000000000000   rbp = 0x0000000000000000
    rsp = 0x00007ffce1c594e8   r12 = 0x0000562ef8a09f40
    r13 = 0x00007ffce1c594f0   r14 = 0x0000000000000000
    r15 = 0x0000000000000000   rip = 0x00007ffce1c594e8
    Found by: call frame info

Loaded modules:
0x562ef8a02000 - 0x562ef8a1cfff  main  ???  (main)
0x7fd5c6a36000 - 0x7fd5c6ae9fff  libm.so.6  ???
0x7fd5c6b85000 - 0x7fd5c6d1efff  libc.so.6  ???
0x7fd5c6d77000 - 0x7fd5c6d8dfff  libpthread.so.0  ???
0x7fd5c6d9a000 - 0x7fd5c6daefff  libgcc_s.so.1  ???
0x7fd5c6db5000 - 0x7fd5c6f3bfff  libstdc++.so.6  ???
0x7fd5c6fa8000 - 0x7fd5c6fcbfff  ld-linux-x86-64.so.2  ???
0x7ffce1c8a000 - 0x7ffce1c8bfff  linux-gate.so  ???
2022-09-07 17:02:29: minidump.cc:5288: INFO: Minidump closing minidump
```