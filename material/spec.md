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
// 带参数的格式。标题 : key1[value1], keyX[valueX]
LOG_DEBUG_FMT("result : code[%s], length[%d]", code.c_str(), 100);
```

### 命名
* 整体采用驼峰命名

| 项 | 规则 | 示例 |
| - | - | - |
| 枚举和值 | 大写底横 | EAT_TIME |
| namespace | 小写 | product |
| 类 | 首字大写驼峰 | Person |
| 函数 | 首字小写驼峰 | eatLunch |
| 变量 | 首字小写驼峰+底横 | lunchNumber_ |

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

namespace product {
class Person {
 public:
  Person();
  ~Person();

  int eat(int number);

 protected:
  int eatLunch(int number);
  int lunchNumber_;

 private:
  int eatDinner(int number);
  int dinnerNumber_;
};
```

## 自研库及其使用
* 目录结构

```
库名称
  inc/库名称/    // 头文件
  src           // 源文件
  test          // 单元测试
  example       // 测试demo
```

* 库内cpp文件加载头文件：#include "库名称/CustomTimer.h"
* 同一个工程内其他项目使用：include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../库名称/inc)

## 文档编写
* [Doxygen](/third/doxygen)

## 代码提交规范
1. 代码提交前必须格式化
1. 代码格式化的单独提交，不要和业务混在一起
1. 在README.md记录修改内容，比如新增目录的用途
1. 配套的UT代码

## 资料
* [单元测试](../dev/ut)
* [clang-format](../third/clang-format)
* [editorconfig](https://juejin.im/post/5b9cba4c6fb9a05cf67a79a4)
