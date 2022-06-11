# CMAKE
* [CMAKE介绍](https://www.hahack.com/codes/cmake/)
* 本文档默认适用Linux，Windows适用的会注明

## 执行使用
* https://cmake.org/cmake/help/v3.14/manual/cmake.1.html
* 语法：cmake -S 代码目录 -B build目录 -G 编译软件 -D参数名称=参数值，示例如下：
  * cmake -S . -B build -DCMAKE_BUILD_TYPE=Debug -DCMAKE_INSTALL_PREFIX=/opt/XYZ // 源代码在当前目录，build过程文件在build目录。推荐用独立目录build存放过程，避免污染源代码目录
  * cd build && clear && make -j$((`nproc`+1)) // 在build目录构建
* cmake过程文件清理：find . -name CMakeFiles | xargs rm -rf && find . -name cmake_install.cmake | xargs rm -rf && find . -name CMakeCache.txt | xargs rm -rf && find . -name Makefile | xargs rm -rf

## 参数
| 名称 | 编码 | 值 | 说明 |
| :-: | - | - | - |
| 编译bit | CMAKE_GENERATOR_PLATFORM | x86 x64 | -DCMAKE_GENERATOR_PLATFORM=x64 |
| 编译类型 | CMAKE_BUILD_TYPE | Debug Release RelWithDebInfo MinSizeRel | -DCMAKE_BUILD_TYPE=Debug |
| 动态库编译 | BUILD_SHARED_LIBS | boolean | -DBUILD_SHARED_LIBS=ON，OFF是静态库 |

## CMakeLists.txt编写
* [CMakeLists.txt编写常用命令](https://www.cnblogs.com/xl2432/p/11225276.html)
* 指令用小写

### 常用变量
* CMake

| 名称 | 编码 |
| :-: | - |
| 源代码目录 | CMAKE_CURRENT_SOURCE_DIR |
| build文件目录 | CMAKE_BINARY_DIR |
| 项目文件目录 | PROJECT_BINARY_DIR |
| 输出执行文件目录 | EXECUTABLE_OUTPUT_PATH |
| 输出库目录 | LIBRARY_OUTPUT_PATH |

* [conanbuildinfo.cmake里的变量](https://docs.conan.io/en/latest/reference/generators/cmake.html)

### 常用语句
```
# conan支持
include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

# 加载conan头文件
include_directories(${CONAN_INCLUDE_DIRS})
message(STATUS "conan include path : ${CONAN_INCLUDE_DIRS}")

# 链接conan库
conan_target_link_libraries(${TARGET_NAME})

if (${CMAKE_BUILD_TYPE} MATCHES Release)
    foreach(_LIB ${CONAN_LIBS_RELEASE})
        target_link_libraries(${TARGET_NAME} ${_LIB})
    endforeach()
else()
    foreach(_LIB ${CONAN_LIBS_DEBUG})
        target_link_libraries(${TARGET_NAME} ${_LIB})
    endforeach()
endif()

# 编译类型判断
if (${CMAKE_BUILD_TYPE} MATCHES Debug)
  message(STATUS "build type is debug")
else()
endif()

# 编译类型参数设置
set(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb")
set(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")

# 生成执行程序
add_executable(${TARGET_NAME} ${SOURCE_FILES})

# [生成动态库静态库](https://www.cnblogs.com/zhoug2020/p/5904206.html)
add_library(${TARGET_NAME} SHARED ${SOURCE_FILES}) // 动态库
add_library(${TARGET_NAME}_static STATIC ${SOURCE_FILES}) // 静态库

set_target_properties(${TARGET_NAME} PROPERTIES CLEAN_DIRECT_OUTPUT 1)
set_target_properties(${TARGET_NAME}_static PROPERTIES OUTPUT_NAME "hello") // 重命名生成名称成hello
set_target_properties(${TARGET_NAME}_static PROPERTIES CLEAN_DIRECT_OUTPUT 1)

# 设置debug库后缀加D，比如%name%D.so
set(CMAKE_DEBUG_POSTFIX D)

# 设置Windows的PDB文件输出路径
set(COMPILE_PDB_OUTPUT_DIRECTORY "")

# 设置输出路径
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

# option
option(ENABLE_VALGRIND_TEST "Set CMAKE_CXX_FLAGS for valgrind use" OFF)
if(ENABLE_VALGRIND_TEST)
endif()

# 设置多个目录的cpp文件
file(GLOB SOURCE_FILES *.cc *.cpp *.cxx)
aux_source_directory(src SOURCE_FILES)
aux_source_directory(test SOURCE_FILES)

# [安装到指定目录](https://blog.csdn.net/qq_38410730/article/details/102837401)
install(TARGETS hello hello_static LIBRARY DESTINATION lib ARCHIVE DESTINATION lib) // 安装库(hello hello_static)到目录lib
install(FILES hello.h DESTINATION include/hello) // 安装hello.h到目录include/hello/

# find_library引入类库
find_library(Z_LIB libz.so.1.2.9 /usr/local/lib/) // 变量名，需引入的库，查询路径
message(STATUS "Z_LIB lib path:" ${Z_LIB})
if(NOT Z_LIB)
    message(FATAL_ERROR "not find the Z_LIB" )
endif()
target_link_libraries(${TARGET_NAME} ${Z_LIB})

# [find_package引入外部依赖包](https://zhuanlan.zhihu.com/p/97369704)
变量：<LibaryName>_FOUND, <LibaryName>_INCLUDE_DIR, <LibaryName>_LIBRARIES
find_package(CURL)
if(CURL_FOUND)
    target_include_directories(${TARGET_NAME} PRIVATE ${CURL_INCLUDE_DIR})
    target_link_libraries(TARGET_NAME} ${CURL_LIBRARIES})
else()
    message(FATAL_ERROR ”CURL library not found”)
endif()
```

### 语法
```
# 加载头文件目录
include_directories(directory1 directory2 ...)

# 加载库目录
link_directories(directory1 directory2 ...)

# 链接具体的库，可以是库名称和库文件名(优先链接动态库)，也可以是文件路径
target_link_libraries(${TARGET_NAME} log4cplus) # 默认优先链接动态库
target_link_libraries(${TARGET_NAME} liblog4cplus.so) # 显示指定链接静态库
target_link_libraries(${TARGET_NAME} liblog4cplus.a) # 显示指定链接动态库
target_link_libraries(${TARGET_NAME} /usr/local/lib/liblog4cplus.so)

# 添加子目录，子目录是库、测试程序或者是install用途。子目录是库的一部分代码就不要用add_subdirectory
add_subdirectory(test)

# 变量操作
set(ABC_BUILD ON)
set(MSG "hi")
set(MSG_TXT "${MSG} world")

# 提示信息
message(STATUS "SOURCE_FILES = ${SOURCE_FILES}.")

# 列表项操作
set(SOURCE_FILES a.cpp b.cpp)
list(APPEND SOURCE_FILES c.cpp)
list(REMOVE_ITEM SOURCE_FILES a.cpp))

# 字符串合并
string(CONCAT TARGET_STRING "x" "y" "z") # TARGET_STRING是xyz

# 文件存在判断
if(EXISTS "${ROOT}/file.xml")
   ...
else()
   ...
endif()
```

## 资料
* [cmake使用示例与整理总结](https://blog.csdn.net/wzzfeitian/article/details/40963457)
* [cmake常用工程示例大集合](https://blog.csdn.net/FreeApe/article/details/52567087)
* [cmake函数、宏和模块](https://www.cnblogs.com/zhoug2020/p/13659952.html)
* [cmake命令速查手册](https://blog.csdn.net/u010552731/article/details/89293101)
* [cmake指令详解](https://blog.csdn.net/bytxl/article/details/50635016)
* [CMAKE自定义模块](https://www.kancloud.cn/itfanr/cmake-practice/82991)
* [列表操作](https://blog.csdn.net/fuyajun01/article/details/9036477)
* [使用execute_process调用shell命令或脚本](https://blog.csdn.net/qq_28584889/article/details/97758450)

### 指令
* [configure_file](https://www.cnblogs.com/the-capricornus/p/4717566.html)
* [generate_export_header](https://www.bookset.io/read/CMake-Cookbook/content-chapter10-10.2-chinese.md)：生成动态库预处理时符号可见性设置的宏
  1. 动态库只公开最小的符号，设置动态库的符号可见性。最好的方式是在默认情况下隐藏所有符号，显式地只公开那些需要使用的符号。
  1. 让应用程序二进制接口(ABI)和库接口(API)一致。

### 安装升级
* [二进制安装](https://blog.csdn.net/freemote/article/details/103454801)
* 源码安装
```
which cmake
wget -O cmake-3.14.0.tar.gz https://github.com/Kitware/CMake/releases/download/v3.14.0/cmake-3.14.0.tar.gz
tar zxvf cmake-3.14.0.tar.gz && cd cmake-3.14.0
cmake -DCMAKE_INSTALL_PREFIX=/usr/local -DCMAKE_USE_OPENSSL=OFF .
make -j$((`nproc`+1)) && make install
cd .. && rm -rf cmake-3.14.0 && rm cmake-3.14.0.tar.gz
```
