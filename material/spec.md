# 开发规范
## 文件
1. 编码是UTF8
1. LineEnd是LF，文件批量修改用dos2unix

## 格式化
* 工具是clang-format，包含有多种[style(风格)](https://blog.csdn.net/booksyhay/article/details/121115665)
* 风格选择[Google](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/contents/)
  * 160个字符换行

## 编码
### 头文件加载
1. 头文件include一次用"#pragma once"
1. 项目头文件用""记载，其他头文件用<>
1. 加载第三方库的头文件用<log4cplus/logger.h>，开始是库名称，如log4cplus
1. 加载顺序和空行见示例

### log
```
// 定义
#include "common/Logger.h"
IMPLEMENT_LOGGER(ClassName)

// 不带参数的格式。标题
LOG_INFO("ABC finish");
// 带参数的格式。标题 : key1[value1], keyX[valueX]。因log库不同可能会有不同写法：
LOG_INFO_FMT("result : code[{}], length[{}]", code, 100);
```

### 划重点
* 函数不要大于40行(不含注释空行等)
* 禁止在头文件实现函数，template函数只能在头文件实现
* 类里有指针成员变量的需在析构函数内delete。内存泄漏见[valgrind](../third/valgrind)
* 尽量用智能指针
* 少用static的函数和变量，多用类和成员变量
* 用namespace
* 库对外的开放
  * 对外提供的业务库，1个库尽量对外只需要1个头文件。这个头文件的类不要有逻辑，只是入口来调用其他类。允许有数据结构定义的其他头文件
  * 内部库(如通用库)尽量遵守上条规则
* 对外的API函数都需有返回码，函数的return是返回码(int)

### 命名
* 整体采用驼峰命名

| 项 | 规则 | 示例 |
| - | - | - |
| 定义相关 |  |  |
| namespace | 首字小写驼峰，多个单词尽量分多层namespace | alg |
| typedef | 首字大写驼峰 | ThreeDimMaskType |
| 结构相关 |  |  |
| 枚举和值 | 大写底横 | EAT_TIME |
| 结构 | 首字大写驼峰 | RequestDto |
| 结构变量 | 首字小写驼峰 | requestId |
| 类 | 首字大写驼峰 | Person |
| 类函数 | 首字小写驼峰，动词在前 | sayHello |
| 类变量 | 首字小写驼峰+底横 | lunchNumber_ |
| 文件系统 |  |  |
| 文件夹 | 首字小写驼峰，多个单词尽量分多层目录 | alg |
| 文件 | 同类名 | Person |

### 示例
```
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

namespace alg {
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

## 自研库及其使用
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

## 文档编写
* 注释见[Doxygen](/third/doxygen)

## 代码提交规范
1. 代码提交前必须格式化
1. 大批量代码的格式化需单独提交，不要和业务混在一起
1. 在README.md记录修改内容，比如新增目录的用途
1. 配套的UT代码

## 资料
* [单元测试](../dev/ut)
* [clang-format](../third/clang-format)
* [editorconfig](https://juejin.im/post/5b9cba4c6fb9a05cf67a79a4)
