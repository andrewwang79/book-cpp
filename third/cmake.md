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

```

### 编译类型
四种：Debug Release RelWithDebInfo MinSizeRel
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
```
INSTALL (TARGETS hello hello_static LIBRARY DESTINATION lib ARCHIVE DESTINATION lib)
INSTALL (FILES hello.h DESTINATION include/hello)
```

### 加入conan支持
```
编辑“CMakeLists.txt”
include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

foreach(_LIB ${CONAN_LIBS_RELEASE})
  target_link_libraries(widget optimized ${_LIB})
endforeach()
```

## 资料
* [CMakeLists.txt编写常用命令](https://www.cnblogs.com/xl2432/p/11225276.html)
* [CMAKE介绍](https://www.hahack.com/codes/cmake/)
* [cmake使用示例与整理总结](https://blog.csdn.net/wzzfeitian/article/details/40963457)
* [cmake常用工程示例大集合](https://blog.csdn.net/FreeApe/article/details/52567087)

* [cmake命令速查手册](https://blog.csdn.net/u010552731/article/details/89293101)
* [cmake指令详解](https://blog.csdn.net/bytxl/article/details/50635016)
* [CMAKE自定义模块](https://www.kancloud.cn/itfanr/cmake-practice/82991)
* [列表操作](https://blog.csdn.net/fuyajun01/article/details/9036477)

### 指令
* [configure_file](https://www.cnblogs.com/the-capricornus/p/4717566.html)
* [generate_export_header](https://www.bookset.io/read/CMake-Cookbook/content-chapter10-10.2-chinese.md)

### 安装升级
* https://blog.csdn.net/freemote/article/details/103454801
