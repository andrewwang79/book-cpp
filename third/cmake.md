# CMAKE
## 调用
* https://cmake.org/cmake/help/v3.14/manual/cmake.1.html
* cmake -S 代码目录 -B build目录 -G 编译软件 -D参数名称=参数值

## 参数
| 参数 | 编码 | 值 | 说明 |
| :-: | - | - | - |
| 编译bit | CMAKE_GENERATOR_PLATFORM | x86 x64 | -DCMAKE_GENERATOR_PLATFORM=x64 |
| 编译类型 | CMAKE_BUILD_TYPE | Debug Release RelWithDebInfo MinSizeRel | -DCMAKE_BUILD_TYPE=Debug |
| 动态库编译 | BUILD_SHARED_LIBS | boolean | -DBUILD_SHARED_LIBS=true |

## 用法
```
// 链接的头文件目录
include_directories(directory1 directory2 ...)

// 链接的库目录
link_directories(directory1 directory2 ...)

// 链接具体的库，可以是库名称和库文件名(优先链接动态库)，也可以是文件路径
# target_link_libraries(${TARGET_NAME} log4cplus liblog4cplus.so /usr/local/lib/liblog4cplus.so)

// 提示信息
MESSAGE( STATUS "SOURCE_FILES = ${SOURCE_FILES}.")

// 移除列表项
list(REMOVE_ITEM ${CONAN_LIBS_RELEASE} "aaa")

// 设置Windows的PDB文件输出路径
set(COMPILE_PDB_OUTPUT_DIRECTORY "")

 // 设置库文件输出路径，cmake不同版本不一样
set(LIBRARY_OUTPUT_PATH "")

// 编译类型判断
IF (CMAKE_BUILD_TYPE STREQUAL Debug)
ELSE()
ENDIF()

// 文件存在判断
if(EXISTS "${ROOT}/file.xml")
   ...
else()
   ...
endif()

// 设置debug库后缀是D
set(CMAKE_DEBUG_POSTFIX D)

// 获取多个目录的cpp文件
file(GLOB PROJECT_SOURCE_FILES *.cc *.cpp *.cxx)
aux_source_directory(src PROJECT_SOURCE_FILES)
aux_source_directory(test PROJECT_SOURCE_FILES)
```
### 常用目录
```
CMAKE_BINARY_DIR：build文件目录
```

### 编译类型
四种：Debug RelWithDebInfo Release MinSizeRel
```
设置方式：
cmake -DCMAKE_BUILD_TYPE=Debug .
SET(CMAKE_BUILD_TYPE "Debug”)

相关参数设置：
SET(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb")
SET(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")
```

## [动态库静态库](https://www.cnblogs.com/zhoug2020/p/5904206.html)
```
设置方式：
cmake -DBUILD_SHARED_LIBS=true
set(BUILD_SHARED_LIBS ON)

同时生成：
ADD_LIBRARY (hello SHARED ${LIBHELLO_SRC})
ADD_LIBRARY (hello_static STATIC ${LIBHELLO_SRC})
SET_TARGET_PROPERTIES (hello_static PROPERTIES OUTPUT_NAME "hello")
SET_TARGET_PROPERTIES (hello_static PROPERTIES CLEAN_DIRECT_OUTPUT 1)
SET_TARGET_PROPERTIES (hello PROPERTIES CLEAN_DIRECT_OUTPUT 1)

```

### 安装到指定目录
* https://blog.csdn.net/qq_38410730/article/details/102837401
```
INSTALL (TARGETS hello hello_static LIBRARY DESTINATION lib ARCHIVE DESTINATION lib)
INSTALL (FILES hello.h DESTINATION include/hello)
```

### 加入conan支持
```
编辑“CMakeLists.txt”
include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

conan_target_link_libraries(${TARGET_NAME})
```

## 资料
* [CMakeLists.txt编写常用命令](https://www.cnblogs.com/xl2432/p/11225276.html)
* [CMAKE介绍](https://www.hahack.com/codes/cmake/)
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
* [generate_export_header](https://www.bookset.io/read/CMake-Cookbook/content-chapter10-10.2-chinese.md)

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
