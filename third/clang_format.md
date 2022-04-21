# clang-format

## 资料
* [.clang-format文件常用参数](https://bugwz.com/2019/01/08/clang-format/)

## 工具
### VisualStudioCode
* 安装插件"Clang-Format"
* 设置 :
* [文件保存时自动格式化](https://www.mycode.net.cn/tools/3064.html)
* [vscode批量格式化代码](https://blog.csdn.net/koukouwuwu/article/details/111879677) : 插件"Format Files"

### VisualStudio
* [VisualStudio](https://docs.microsoft.com/zh-cn/cpp/ide/writing-and-refactoring-code-cpp?view=msvc-170)
* 设置 : 工具 -> 选项 -> 文本编辑器 -> C/C++ -> 格式设置 -> 默认格式设置样式 ->

## 文件
* settings.json
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

* .clang-format
```
# Run manually to reformat a file:
# clang-format -i --style=file <file>
Language:       Cpp
BasedOnStyle:   Google
ColumnLimit:    160
AllowShortBlocksOnASingleLine: true
DerivePointerAlignment: false
PointerAlignment: Left
```
