# 大规模系统的结构
## 目录结构
```
prj/
├── doc/                          # 文档
├── CMakeLists.txt                # 主CMake配置
├── cmake/                        # CMake模块和工具
│   ├── FindXXX.cmake             # 自定义查找模块
├── cicd/                         # CI/CD 配置
│   ├── jenkins/                  # Jenkins流水线脚本
│   └── scripts/                  # 构建脚本
├── include/                      # 公共头文件
│   └── prj/                      # 项目命名空间
│       └── module/               # 模块
├── src/                          # 源代码
│   ├── module/                   # 模块
│   └── main.cpp                  # 主程序入口
├── test/                         # 测试代码
├── examples/                     # 使用示例
├── tools/                        # 开发工具
└── data/                         # 示例数据
```

## 头文件
### 使用方法
| 类型 | 存放路径 |
| - | - |
| 接口头文件 | include |
| 私有头文件 | src |

* 库和执行程序都适用路径规则
* 库的include要对外开放
* 执行文件没有接口头文件，所以都是PRIVATE。
    1. 可以只用src
    1. 如果用include则要兼容src(如QT自动生成的文件路径在src下)

```
# CMakeLists.txt
target_include_directories(prj
    PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>  # 公共头文件
    PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src
)
```

### 头文件从私有变成接口
> 有些情况下头文件要对外开放

* 私有接口
```
#include "module/MyClass.h"  // 使用方法：直接包含src路径

prj/
├── include/
└── src/
    ├── module/
    │   ├── MyClass.h       # 私有头文件（当前不公开）
    │   └── MyClass.cpp
```

* 公共接口
```
#include "prj/module/MyClass.h"  // 使用方法：增加项目命令空间

prj/
├── include/
│   └── prj/
│       ├── module/
│       │   └── MyClass.h   # 接口头文件（位置改变）
└── src/
    ├── module/
    │   └── MyClass.cpp     # 实现文件位置不变
```