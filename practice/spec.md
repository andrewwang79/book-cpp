# 开发规范
## 文件
1. 编码是UTF8
1. LineEnd是LF，文件批量修改用dos2unix

## 格式化
* 工具是clang-format，包含有多种[style(风格)](https://blog.csdn.net/booksyhay/article/details/121115665)
* 风格选择[Google](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/contents/)
  * 160个字符换行

## 代码
* https://google.github.io/styleguide/cppguide.html

### 头文件
1. 头文件唯一加载用"#pragma once"
1. 加载项目的头文件用""。加载依赖库的头文件用<log4cplus/logger.h>，开始是库名称，如log4cplus
1. 尽量在cpp文件include头文件，而不是在头文件include
1. 不在头文件using namespace
1. 尽量不使用前置声明（forward declarations）
1. 加载顺序和空行见示例
1. 禁止在头文件实现函数(template函数只能在头文件实现)，函数的定义和默认值定义在头文件

### 类
1. 禁止构造/析构函数调用虚函数
1. 类里有指针成员变量的需在析构函数内delete。内存泄漏见[valgrind](../third/valgrind)
1. 拷贝/赋值函数在不需要时，应显式删除
1. 类禁止使用public的成员变量。仅有成员变量的结构使用struct

### 函数
1. 函数及其默认值在头文件中定义
1. 函数参数顺序：输出参数在前，输入参数在后。输入参数用const定义【函数参数的压栈顺序是从右至左】
1. 不在头文件实现函数(template函数除外)
1. 单个函数逻辑不要过于复杂，不超过80行(不含注释空行等)
1. 新增函数需编写配套的单元测试代码

### 变量
1. 变量需初始化
1. 禁止使用隐式类型转换
1. 最小作用域
1. 尽量少用变量，特别是局部变量
1. 变量名称必须有意义，不要用魔数
1. 使用constexpr初始化常量
1. 仅在定义局部变量时使用auto

### 代码和注释
1. 重点业务函数出入口需有日志
1. 体现的是业务逻辑，而不是只是代码和注释
    1. 代码：用内联函数或宏封装业务逻辑，而不是一行行代码(循环加1)
    1. 注释：一段业务逻辑(如FIFO/LRU)，而不是一行行注释(循环加1)
1. 尽量用代码说清楚业务逻辑，减少注释
1. 使用doxygen风格编写注释，详见[Doxygen](/third/doxygen)
1. 每个文件有版权公告，类和函数都有注释

#### log格式
```c++
// 定义
#include "common/Logger.h"
IMPLEMENT_LOGGER(ClassName)

// 不带参数的格式。标题
LOG_INFO("ABC finish");
// 带参数的格式。标题 : key1[value1], keyX[valueX]。因log库不同可能会有不同写法：
LOG_INFO_FMT("result : code[{}], length[{}]", code, 100);
```

#### 注释
```
/**
 * @brief  函数简介，比如功能说明
 * @date   2021.05.12
 * @author wangyaqi
 * @param
 * | 名称 | 字段 | 类型 | 必须 | 说明 |
 * | :-: | - | - | - | - |
 * | 类型 | type | string | Y | 类型枚举 |
 * | 序列号 | sn | string | Y | 唯一 |
 * | 引用字段A[out] | refA | string& | Y | 说明 |
 * @return 返回值说明
 *   @retval 0 成功
 *   @retval -1 失败
 * @exception 超出范围的异常
 * @warning   警告
 * @attention 注意事项
 * @note 备注说明。可用于函数修改记录
 * | 时间 | 作者 | 说明 |
 * | :-: | - | - |
 * | 2021.05.12 | wangyaqi | 增加输入参数xxx |
 */
```

### 命名
* 整体采用驼峰命名

| 列表 | 项 | 规则 | 示例 |
| - | - | - | - |
| 定义 | namespace | 首字小写驼峰，多词尽量用多层namespace | cn.abc |
| 定义 | typedef | 同"类" | ThreeDimMaskType |
| 定义 | 宏 | 全大写底横 | IS_NULL |
| 结构 | 枚举，枚举值 | 同"宏" | EAT_TIME |
| 结构 | 类 | 首字大写驼峰 | RequestDto |
| 结构 | 类-函数 | 首字小写驼峰，**动词在前** | sayHello |
| 结构 | 类-公有变量 | 首字小写驼峰 | lunchNumber |
| 结构 | 类-私有变量 | 首字小写驼峰+底横 | lunchNumber_ |
| 文件系统 | 目录 | 同"namespace" |  |
| 文件系统 | 文件 | 同"类" |  |
| 交付物 | 库/执行文件 | 同"namespace" |  |

* 示例

```c++
#pragma once

#include <memory>
#include <log4cplus/logger.h>

#include "common/StringUtil.h"

enum EAT_TIME {
  MORNING,
  NOON
};

using ThreeDimMaskType = itk::Image<uint8_t, 3>;
typedef itk::ImageDuplicator<ThreeDimMaskType> ThreeDuplicatorType;

struct RequestDto {
  int requestId;
  string value;
}

namespace cn.abc {
class Person {
 public:
  Person();
  ~Person();

  int sayHello();

 protected:
  string setName(string name);
  string name_;

 private:
  int getAge();
  int age_;
};
```

### 导出的业务接口
1. 业务接口的导出头文件不应包含任何第三方库头文件，尽量少包含内部头文件
1. 业务接口使用返回码定义返回结果

### misc
1. 代码应定义在命名空间中
1. 函数尽量写成类的成员函数
1. 尽量多用类，非必要不要用全局函数
1. 尽量用智能指针
1. 业务尽量少用static，通用尽量多用static
1. 合理使用友元类及函数

## 库
### 库对外发布
1. 依赖库和逻辑不暴露，减少依赖冲突风险(比如boost尽量不暴露)
    * 1个库尽量对外只需要1个头文件。这个头文件的类不要有逻辑，只是入口来调用其他类。允许有数据结构定义的其他头文件
    * 内部库(如通用库)尽量遵守上条规则
1. API需有返回码(int)
1. 依赖库(不含HEAD ONLY的库)在调用方和提供方统一的方法
    1. 调用方和提供方使用同一个库(可以各自编译，参数一样)
    1. 提供方提供依赖库目录，调用方设置成库链接路径
    1. 提供方将依赖库代码集成到提供库内

### 库发布头文件加载路径方案

| 项 | 说明 | 优势 | 劣势 |
| - | - | - | - |
| 方案1 | 用宏定义区分头文件加载的路径 | 灵活度高 | 头文件管理麻烦，需自己处理 |
| 方案1 | 头文件目录(include)下加同项目的目录结构 | 头文件管理方便(按规则即可) | include目录层级多 |

### 对外目录结构
```
lib1
  module1
    header1.h
  module2
    submodule21
      header21.h
```

### CMake和头文件写法
* CMakeLists.txt
  * 定义宏LIB1_EXPORT（使用方不用定义宏）
* 对外头文件
```
#ifdef LIB1_EXPORT
  #include 内部规则的路径
#else
  #include 外部规则的路径
#endif
```

### 库结构及其引用
* 目录结构

```
库名称
  include/库名称/    // 头文件
  src               // 源文件
  test              // 单元测试
  example           // 测试demo
```

* 库内cpp文件加载头文件：#include "库名称/Timer.h"
* 同一个工程内其他项目使用：include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../库名称/include)

## 代码提交
1. 代码提交前必须格式化
1. 大批量代码的格式化需单独提交，不要和业务混在一起
1. 在README.md记录修改内容，比如新增目录的用途
1. 配套的UT代码

## 资料
* [单元测试](../dev/ut)
* [clang-format](../third/spec)
* [editorconfig](https://juejin.im/post/5b9cba4c6fb9a05cf67a79a4)
