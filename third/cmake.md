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
* add_subdirectory：subdirector相当于嵌套函数；下级函数继承上级函数的所有参数，下级函数修改的(上级函数也有)参数不会生效到上级函数

### 常用变量
* CMake

| 名称 | 编码 |
| - | - |
| 编译类型 | CMAKE_BUILD_TYPE |
| 最外层CMakeLists.txt所在目录 | CMAKE_SOURCE_DIR |
| build目录 | CMAKE_BINARY_DIR |
| 项目文件目录 | PROJECT_BINARY_DIR |
| 执行文件输出目录 | EXECUTABLE_OUTPUT_PATH |
| 库文件输出目录 | LIBRARY_OUTPUT_PATH |
| cmake当前所处的源代码目录 | CMAKE_CURRENT_SOURCE_DIR |
| cmake当前所处的build目录 | CMAKE_CURRENT_BINARY_DIR |
| 操作系统 | CMAKE_SYSTEM_NAME |
| 操作系统BOOL值 | WIN32, UNIX |

* [conanbuildinfo.cmake里的变量](https://docs.conan.io/en/latest/reference/generators/cmake.html)

| 名称 | 编码 |
| :-: | - |
| 头文件 | CONAN_INCLUDE_DIRS |
| release库 | CONAN_LIBS_RELEASE |
| debug库 | CONAN_LIBS_DEBUG |
| 具体库(config)的bin目录 | CONAN_BIN_DIRS_LIBCONFIG |

### 常用语句
```
# conan支持
include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

# 加载conan头文件
include_directories(${CONAN_INCLUDE_DIRS})
message(STATUS "conan include path : ${CONAN_INCLUDE_DIRS}")

# 链接conan库
conan_target_link_libraries(${TARGET_NAME}) // 如能自动区分Debug/Release，则废弃下面的语句

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

# option(只在cmake生效)
option(ENABLE_VALGRIND_TEST "Set CMAKE_CXX_FLAGS for valgrind use" OFF)
if(ENABLE_VALGRIND_TEST)
endif()

# 源代码的宏定义
add_definitions(-DABC=1)
源代码里使用：
#ifdef ABC
#endif

# 目录文件放到列表变量里
file(GLOB_RECURSE SOURCE_FILES *.cpp) // 递归当前目录下符合后缀的文件放到列表SOURCE_FILES
file(GLOB SOURCE_FILES *.cc *.cpp *.cxx) // 当前目录下符合后缀的文件放到列表SOURCE_FILES
aux_source_directory(src SOURCE_FILES) // 根目录/src下符合默认后缀的文件放到列表SOURCE_FILE

# [安装到指定目录](https://blog.csdn.net/qq_38410730/article/details/102837401), https://blog.csdn.net/qq_38410730/article/details/102837401
install(TARGETS hello hello_static LIBRARY DESTINATION lib ARCHIVE DESTINATION lib) // 安装库(hello hello_static)到目录lib
install(FILES hello.h DESTINATION include/hello) // 安装hello.h到目录include/hello/
install(DIRECTORY /opt/lib/ DESTINATION abc/) // 因为有"/"，abc目录下有lib目录下的内容，但没有lib目录本身
install(DIRECTORY /opt/lib DESTINATION abc/) // abc目录下有lib目录

# 文件复制
file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/config/ DESTINATION config) // 目录复制
file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/abc.cfg DESTINATION config) // 单文件复制
// 复制特定后缀的文件列表，如果是【${CMAKE_CURRENT_SOURCE_DIR}/*】则复制目录下的所有(含所有子目录)
file(GLOB cfgFiles "${CMAKE_CURRENT_SOURCE_DIR}/*.cfg")
file(COPY ${cfgFiles} DESTINATION config)

# find_library引入类库
find_library(Z_LIB libz.so.1.2.9 /usr/local/lib/) // 变量名，需引入的库，查询路径
message(STATUS "Z_LIB lib path:" ${Z_LIB})
if(NOT Z_LIB)
    message(FATAL_ERROR "not find the Z_LIB" )
endif()
target_link_libraries(${TARGET_NAME} ${Z_LIB})

# 字符串
* [字符串操作](https://blog.csdn.net/m0_57845572/article/details/118520561)
string(TOLOWER ${CMAKE_BUILD_TYPE} _CMAKE_BUILD_TYPE) # 大小写
string(CONCAT TARGET_STRING "x" "y" "z") # 合并，TARGET_STRING是xyz

# 编译参数查看
message("CMAKE_C_FLAGS_DEBUG is ${CMAKE_C_FLAGS_DEBUG}")
message("CMAKE_C_FLAGS_RELEASE is ${CMAKE_C_FLAGS_RELEASE}")
message("CMAKE_C_FLAGS_RELWITHDEBINFO is ${CMAKE_C_FLAGS_RELWITHDEBINFO}")
message("CMAKE_C_FLAGS_MINSIZEREL is ${CMAKE_C_FLAGS_MINSIZEREL}")

message("CMAKE_CXX_FLAGS_DEBUG is ${CMAKE_CXX_FLAGS_DEBUG}")
message("CMAKE_CXX_FLAGS_RELEASE is ${CMAKE_CXX_FLAGS_RELEASE}")
message("CMAKE_CXX_FLAGS_RELWITHDEBINFO is ${CMAKE_CXX_FLAGS_RELWITHDEBINFO}")
message("CMAKE_CXX_FLAGS_MINSIZEREL is ${CMAKE_CXX_FLAGS_MINSIZEREL}")


# 链接具体的库，可以是库名称、库文件名和文件全路径
target_link_libraries(${TARGET_NAME} log4cplus) # 库名称，不需要lib前缀，默认优先链接动态库
target_link_libraries(${TARGET_NAME} liblog4cplus.so) # 显式指定链接动态库
target_link_libraries(${TARGET_NAME} liblog4cplus.a) # 显式指定链接静态库
target_link_libraries(${TARGET_NAME} /usr/local/lib/liblog4cplus.so)
target_link_libraries(${TARGET_NAME} A B) # A依赖于B

# 添加子目录，子目录是库、测试程序或者是install用途。子目录是库的一部分代码就不要用add_subdirectory
add_subdirectory(test)

# 变量操作
set(ABC_BUILD ON)
set(MSG "hi")
set(MSG_TXT "${MSG} world")

# 提示信息
message(STATUS "SOURCE_FILES = ${SOURCE_FILES}.")

# 列表项操作
* [列表操作](https://blog.csdn.net/fuyajun01/article/details/9036477)
set(SOURCE_FILES a.cpp b.cpp)         // 创建列表
list(APPEND SOURCE_FILES c.cpp)       // append
list(REMOVE_ITEM SOURCE_FILES a.cpp)  // remove
FOREACH(item ${SOURCE_FILES})         //循环
  message(${item})
ENDFOREACH()

# 文件存在判断
if(EXISTS "${ROOT}/file.xml")
   ...
else()
   ...
endif()

# IF命令 https://blog.csdn.net/weixin_41923935/article/details/122196384
if((NOT condition) AND (condition OR (condition)))

# 调试
return (0) # 退出
```

### 语法
```
# 添加头文件目录
include_directories(directory1 directory2 ...)

# 添加需要链接的库文件目录
link_directories(directory1 directory2 ...)

# 添加需要链接的库文件全路径
link_libraries(full_path)
```

### 全局变量
```
// 父文件
set(GLOBAL_VAR CACHE INTERNAL "") // 全局变量初始化
// 子文件
set(GLOBAL_VAR ${GLOBAL_VAR} "GLOBAL_VALID_CONTENT" CACHE INTERNAL "") // 追加内容，修改会生效到全局变量
set(GLOBAL_VAR ${GLOBAL_VAR} "FILE_VALID_CONTENT" "") // 追加内容，修改只在本文件内有效
aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR} GLOBAL_VAR) // 追加内容，修改只在本文件内有效
```

## CMake编写
* [include](https://blog.csdn.net/qq_38410730/article/details/102677143)
* [cmake函数、宏和模块](https://www.cnblogs.com/zhoug2020/p/13659952.html)
* [CMake中的function和macro](https://blog.csdn.net/fb_941219/article/details/89358576)
* [CMAKE自定义模块](https://www.kancloud.cn/itfanr/cmake-practice/82991)

### 宏
* LIST作为输入参数的使用方法，有两种方法，见fn1和fn2

```
# TARGET_NAME是字符串，LINK_LIBRARIES是LIST。LIST必须用set定义成变量后再作为输入参数使用

# 方法1：
macro(fn1 TARGET_NAME LINK_LIBRARIES)
    target_link_libraries(${TARGET_NAME} ${${LINK_LIBRARIES}})
endmacro()
# 使用
set(LINK_LIBRARIES a b)
fn1("test1" LINK_LIBRARIES)

# 方法2：
macro(fn2 TARGET_NAME LINK_LIBRARIES)
    target_link_libraries(${TARGET_NAME} ${LINK_LIBRARIES})
endmacro()
# 使用
set(LINK_LIBRARIES a b)
fn2("test2" "${LINK_LIBRARIES}") # 加双引号相当于输入1个列表参数，不加双引号相当于输入多个参数。如果这参数是最后一个是可以不加双引号
```

## 资料
* [cmake使用示例与整理总结](https://blog.csdn.net/wzzfeitian/article/details/40963457)
* [cmake常用工程示例大集合](https://blog.csdn.net/FreeApe/article/details/52567087)
* [cmake命令速查手册](https://blog.csdn.net/u010552731/article/details/89293101)
* [CMAKE使用](https://www.swack.cn/wiki/001558681974020669b912b0c994e7090649ac4846e80b2000/001560826762151294a43f838f4423299fec74dd2a0f257000)
* [cmake指令详解](https://blog.csdn.net/bytxl/article/details/50635016)
* [configure_file](https://www.cnblogs.com/the-capricornus/p/4717566.html)

### find_package
* [find_package原理](https://blog.csdn.net/lianshaohua/article/details/108402470)
* [find_package引入外部依赖包](https://zhuanlan.zhihu.com/p/97369704)

```
变量：<LibaryName>_FOUND, <LibaryName>_INCLUDE_DIR, <LibaryName>_LIBRARIES

示例：
find_package(CURL)
if(CURL_FOUND)
    target_include_directories(${TARGET_NAME} PRIVATE ${CURL_INCLUDE_DIR})
    target_link_libraries(TARGET_NAME} ${CURL_LIBRARIES})
else()
    message(FATAL_ERROR ”CURL library not found”)
endif()
```

###  动态库添加版本信息
* [动态库添加版本信息](https://blog.csdn.net/qq295109601/article/details/118063009)

```
# Windows
Version.rc.in(来自VisualStudio) -> Version.rc
add_library(lib SHARED ${SOURCES} ${VERSION_FILES})

# Linux
SET_TARGET_PROPERTIES(lib PROPERTIES VERSION 1.2.3 SOVERSION 1)
```

###  执行外部命令
* [使用execute_process调用shell命令或脚本](https://blog.csdn.net/qq_28584889/article/details/97758450)
* [执行外部命令](https://zhuanlan.zhihu.com/p/396036530)

### 动态库的符号表导出
1. set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON): 全局导出
1.  [cmake符号指定文件导出](https://blog.csdn.net/LaineGates/article/details/83899961), [详细使用](https://www.bookset.io/read/CMake-Cookbook/content-chapter10-10.2-chinese.md)
```
include(GenerateExportHeader)
generate_export_header(message-shared
  BASE_NAME "message"
  EXPORT_MACRO_NAME "message_EXPORT"
  EXPORT_FILE_NAME "${CMAKE_BINARY_DIR}/${INSTALL_INCLUDEDIR}/messageExport.h"
  DEPRECATED_MACRO_NAME "message_DEPRECATED"
  NO_EXPORT_MACRO_NAME "message_NO_EXPORT"
  STATIC_DEFINE "message_STATIC_DEFINE"
  NO_DEPRECATED_MACRO_NAME "message_NO_DEPRECATED"
  DEFINE_NO_DEPRECATED
)
```

### CMAKE_PREFIX_PATH设置
* https://www.cnblogs.com/Fitanium/p/16181605.html
* 命令搜索使用路径 : find_package、find_program、find_library、find_file、find_path
```
cmake  -DCMAKE_PREFIX_PATH="path1;path2"
list(APPEND CMAKE_PREFIX_PATH "path1")
```

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

### 具体命令和性能分析
* 看详细命令 : make VERBOSE=1
* 生成执行CMake过程的性能分析报告 : cmake --profiling-format=google-trace --profiling-output=profile.json
