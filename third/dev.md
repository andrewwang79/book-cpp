# 开发工具

## 工具清单
| 类别 | 工具名 | 说明 |
| :-: | - | - |
| 开发 | VisualStudio/vscode |  |
| 文档生成 | [doxygen](https://www.doxygen.nl/download.html) | Windows是Doxywizard |
| 代码规范和格式化 | [clang-format]() |  |
| 代码规范和格式化 | EditorConfig | [标准属性](https://editorconfig.org/#supported-properties), [vs2019的cpp属性](https://docs.microsoft.com/en-us/visualstudio/ide/cpp-editorconfig-properties?view=vs-2019)无效 |
| 代码规范设计 | 插件EditorConfig Language Service | 格式化不需要本工具 |
| [在线编程](https://coliru.stacked-crooked.com/) | Coliru | 简单测试 |


## 代码规范和格式化
### ClangFormat
* vscode
    1. 保存时自动格式化：配置里启用
    1. 启用c++谷歌代码规范：放置.clang-format

* VisualStudio
    1. 保存时自动格式化：扩展 –> 管理扩展 –> 联机 –> 搜索Format document on Save
    1. 启用c++谷歌代码规范
        * 默认的：勾选：工具 -> 选项 -> 文本编辑器 -> C/C++ -> 默认格式设置样式 -> Google
        * 自定义：[操作](https://blog.csdn.net/qq_33101873/article/details/121426522)，注意如果拷贝.clang-format一定要在资源管理器里操作

### EditorConfig
1. 放置文件[.editorconfig](https://github.com/andrewwang79/cpp.practice/blob/master/.editorconfig)
1. 代码规范配置：取消勾选：工具 -> 选项 -> 文本编辑器 -> C/C++ -> 启用ClangFormat支持

## 文档生成
* [使用](https://www.shangmayuan.com/a/550f0c41e2c14fe89f0d2d58.html), [使用](https://blog.csdn.net/Candy1232009/article/details/80786179)
* [demo](https://github.com/andrewwang79/cpp.practice/tree/master/doxygen)

## 动态代码检查工具
* google的：https://github.com/google/sanitizers
