# clang-format

## 资料
* [.clang-format文件常用参数](https://bugwz.com/2019/01/08/clang-format/)

## VisualStudioCode配置
* 安装插件"Clang-Format"，配置"文件"里的2个文件
* [vscode批量格式化代码](https://blog.csdn.net/koukouwuwu/article/details/111879677) : 插件"Format Files"
* 默认行尾字符(Files:Eol)设置成"\n"

### 文件
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

# VisualStudio
* [VisualStudio](https://docs.microsoft.com/zh-cn/cpp/ide/writing-and-refactoring-code-cpp?view=msvc-170)
* 设置 : 工具 -> 选项 -> 文本编辑器 -> C/C++ -> 格式设置 -> 默认格式设置样式 ->
