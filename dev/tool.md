# 开发工具

## 工具清单
| 类别 | 工具名 | 说明 |
| :-: | - | - |
| 开发 | Visual Studio |  |
| 文档生成 | [doxygen](https://www.doxygen.nl/download.html) | Windows是Doxywizard |
| 代码规范 | EditorConfig | [标准属性](https://editorconfig.org/#supported-properties), [vs2019的cpp属性](https://docs.microsoft.com/en-us/visualstudio/ide/cpp-editorconfig-properties?view=vs-2019)无效 |
| 代码规范设计 | 插件EditorConfig Language Service | 格式化不需要本工具 |

## 代码格式化
### ClangFormat
1. 新建文件是UTF8文件：放置文件[.editorconfig](https://github.com/andrewwang79/cpp.practice/blob/master/.editorconfig)
1. 代码规范配置：勾选：工具 -> 选项 -> 文本编辑器 -> C/C++ -> 默认格式设置样式 -> Google

### EditorConfig
1. 放置文件[.editorconfig](https://github.com/andrewwang79/cpp.practice)
1. 代码规范配置：取消勾选：工具 -> 选项 -> 文本编辑器 -> C/C++ -> 启用ClangFormat支持

## 文档生成
* [使用](https://www.shangmayuan.com/a/550f0c41e2c14fe89f0d2d58.html), [使用](https://blog.csdn.net/Candy1232009/article/details/80786179)
* [demo](https://github.com/andrewwang79/cpp.practice/tree/master/doxygen)
