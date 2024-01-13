# 代码规范和格式化
## VisualStudioCode
* 格式化：安装插件"Clang-Format"，放置"文件"里的2个文件
* [vscode批量格式化代码](https://blog.csdn.net/koukouwuwu/article/details/111879677) : 安装插件"Format Files"
* vscode配置：
  * 默认行尾字符(Files:Eol)设置成"\n"
  * 保存时自动格式化
  * settings.json，自定义程序和配置文件
  ```
    "clang-format.executable": "/usr/bin/clang-format",
    "clang-format.path": "${workspaceFolder}/cicd/.clang-format"
  ```

## VisualStudio
1. 保存时自动格式化：安装插件"Format document on Save"(扩展 –> 管理扩展 –> 联机 –> 搜索)
1. 文件自动UTF8：安装插件"ForceUTF8(No BOM)"
1. 格式化(使用谷歌代码规范)，有两种方法，推荐方法2:
    1. 系统方法：勾选：工具 -> 选项 -> 文本编辑器 -> C/C++ -> 格式设置 -> 默认格式设置样式 -> Google
    2. 自定义方法：[操作](https://blog.csdn.net/qq_33101873/article/details/121426522)，注意如果拷贝文件".clang-format"一定要在资源管理器里操作

## clang-format
### 安装
* apt-get install clang-format // 安装
* [安装包](https://github.com/llvm/llvm-project/releases/tag/llvmorg-10.0.0)
* clang-format --version // 确保所有环境(如Windows/Linux)使用的版本是一致的，比如10

### 使用方法
* [用法详解](https://zhuanlan.zhihu.com/p/641846308)
* [.clang-format文件常用参数](https://bugwz.com/2019/01/08/clang-format/)
* clang-format -i --style=file <file> // 手工格式化，可以看详细信息，查找问题时比较好用
* Windows下脚本 : Get-ChildItem "path\to\your\code" -Recurse -Include *.h, *.cpp | ForEach-Object { clang-format -i $_.FullName }

### 集成使用
* 头文件被其他头文件依赖，必须放在最上面，但是格式化后自动放下面了：通过备注或者加空换行把这个头文件放在最上面，和其他头文件隔离开
```
// FIRST_INCLUDES_BEGIN
#include "dependence.h"
// FIRST_INCLUDES_END
```

* 不需要.clang-format的代码块
```
 /* clang-format off */
 int a = 42;
 a++;
 ...
 /* clang-format on */
```

### 格式化文件
* .clang-format
```
# Run manually to reformat a file:
# clang-format -i --style=file <file>
Language:       Cpp
BasedOnStyle:   Google
ColumnLimit:    200
AllowShortBlocksOnASingleLine: true
DerivePointerAlignment: false
PointerAlignment: Left
```

* .vscode/settings.json
```
{
    "clang-format.language.cpp.fallbackStyle": "Google",
    "clang-format.language.cpp.style": "file",
    "C_Cpp.clang_format_fallbackStyle": "Google",
    "C_Cpp.clang_format_style": "file",
    "C_Cpp.clang_format_sortIncludes": true,
    "editor.formatOnSave": true
}
```

## 资料
* [在 Visual Studio 中编辑和重构 C++ 代码](https://docs.microsoft.com/zh-cn/cpp/ide/writing-and-refactoring-code-cpp?view=msvc-170)

### EditorConfig
1. 放置文件[.editorconfig](https://github.com/andrewwang79/cpp.practice/blob/master/.editorconfig)
1. 代码规范配置：取消勾选：工具 -> 选项 -> 文本编辑器 -> C/C++ -> 启用ClangFormat支持
