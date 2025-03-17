# 返回码规范
* 通用库和业务方都定义和使用
* 不使用msg，所有的详细信息通过data明确返回(便于国际化)
* [返回码枚举](https://sw.wangyaqi.cn/#/sys/api?id=%e8%bf%94%e5%9b%9e%e7%a0%81)

## 使用方法
### 成功
1. 返回RETURNCODE::SUCCESS(成功返回码)
### 失败
1. 非最外层程序调用失败：抛出异常ReturnCodeException并记录异常信息。
```
THROW_RETURNCODE(RETURNCODE::NO_FILE);
```
1. 最外层程序调用失败:捕获异常情况并返回错误码。
```
catch (ReturnCodeException& e) {  
    return e.getCode();  
  }
```

## 系统的返回码分配示例
### 子系统分段
| 子系统  | 范围 | 说明 |
| - | - | - |
| 通用 | [0, 1000) |  |
| 业务A | [100000, 200000) | 每个模块10000个 <br> 通用模块 : [100000, 101000) <br> 业务模块1 : [120000, 130000) |

### 业务定义
* 1个返回码头文件"业务AReturncode.h"
```
class 业务A_RETURNCODE {
  class COMMON {
    REGISTER_RETURNCODE(RC1, 100000);
    REGISTER_RETURNCODE(RC2, 100001);
  };
  class 业务模块1 {
    REGISTER_RETURNCODE(RC1, 120000);
    REGISTER_RETURNCODE(RC2, 120001);
  };
};
```