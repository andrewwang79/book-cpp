# 开发工具

## 工具清单
| 类别 | 工具名 | 说明 |
| :-: | - | - |
| 开发 | VisualStudio/vscode |  |
| 文档生成 | [doxygen](./doxygen) | Windows是Doxywizard |
| 代码规范和格式化 | [clang-format](./clang_format) |  |
| 代码规范和格式化 | EditorConfig | [标准属性](https://editorconfig.org/#supported-properties), [vs2019的cpp属性](https://docs.microsoft.com/en-us/visualstudio/ide/cpp-editorconfig-properties?view=vs-2019)无效 |
| 代码规范设计 | 插件EditorConfig Language Service | 格式化不需要本工具 |
| [在线编程](https://coliru.stacked-crooked.com/) | Coliru | 简单测试 |

## 动态代码检查工具
* google的：https://github.com/google/sanitizers

# 包管理工具
> nuget，conan，pip，maven，npm；apt，yum...

1. Nuget构建和使用
1. APT（Advanced Packaging Tool）
    1. APT是Debian和Ubuntu等Linux发行版中使用的包管理器。它可以从软件仓库中下载和安装软件包，也可以升级、卸载已安装的软件包。APT支持依赖关系解决，可以自动安装软件包所需的依赖项。
    1. Linux软件包和(运行环境/编译环境)库管理，[使用方法](/dev/compile?id=%e8%bd%af%e4%bb%b6%e5%8c%85%e9%85%8d%e7%bd%ae%e5%b7%a5%e5%85%b7)
1. YUM（Yellowdog Updater Modified）
    1. Red Hat、CentOS和Fedora等Linux发行版中使用的包管理器。它可以从软件仓库中下载和安装软件包，也可以升级、卸载已安装的软件包。YUM支持依赖关系解决，可以自动安装软件包所需的依赖项。
1. DPKG：dpkg
    1. Debian和Ubuntu等Linux发行版中使用的低级包管理工具。它可以安装、升级和卸载.deb格式的软件包。dpkg还可以查询已安装软件包的信息，例如软件包的版本号、大小和安装日期等。dpkg通常与其他高级包管理器（如APT）一起使用，以提供更高级别的软件包管理功能。
1. RPM（Red Hat Package Manager）
    1. Red Hat、CentOS和Fedora等Linux发行版中使用的低级包管理工具。它可以安装、升级和卸载.rpm格式的软件包。RPM不支持依赖关系解决，需要手动解决依赖项问题。
