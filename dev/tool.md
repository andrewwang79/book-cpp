# 开发工具

## 工具
| 类别 | 工具 | 说明 |
| :-: | - | - |
| 开发工具 | Visual Studio |  |
| 代码规范工具 | EditorConfig | [标准属性](https://editorconfig.org/#supported-properties), [vs2019的cpp属性](https://docs.microsoft.com/en-us/visualstudio/ide/cpp-editorconfig-properties?view=vs-2019)无效 |
| 代码规范设计工具 | 插件EditorConfig Language Service | 格式化不需要本工具 |

## 代码格式化
### ClangFormat
1. 新建文件是UTF8文件：放置文件[.editorconfig](https://github.com/andrewwang79/cpp.practice/blob/master/.editorconfig)
1. 代码规范配置：勾选：工具 -> 选项 -> 文本编辑器 -> C/C++ -> 默认格式设置样式 -> Google

### EditorConfig
1. 放置文件[.editorconfig](https://github.com/andrewwang79/cpp.practice)
1. 代码规范配置：取消勾选：工具 -> 选项 -> 文本编辑器 -> C/C++ -> 启用ClangFormat支持
