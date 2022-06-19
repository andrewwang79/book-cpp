# conan
## 安装
```
Conan的使用需基于Python, 安装方式很简单:
pip install conan=1.17.0
```

## 常用命令
```
conan remote add <server_name> <sever_url> // 将远端conan服务器加入本地列表。conan remote add cloud http://conan.abc.com/artifactory/api/conan/ConanLibraries
conan install . -s arch=x86_64 -s os=Linux -r cloud // Linux初始化
conan install . -s arch=x86_64 -s os=Windows -r cloud // Windows初始化
conan search rapidjson/1.1.0@Common/stable // 查看本地库的详细信息
conan install -r cloud rapidjson/1.1.0@Common/stable // 安装具体包
conan search -r cloud rapidjson/1.1.0@Common/stable -q "arch=x86 AND os=Windows" // 查看远程库的详细信息
conan remove -p packageId // 删除本地库packageId
conan remove -r cloud rapidjson/1.1.0@Common/stable -q "arch=x86_64 AND os=Linux" --force // 强制删除远程库

conan export-pkg conanfile.py VTK/8.2.0@Common/stable-debug1 -s arch=x86_64 -s os=Linux -s build_type=Debug -f // 本地打包
conan upload VTK/8.2.0@Common/stable -r cloud --force --all // 上传提交
```

## conanfile.txt内容
```
[requires]
rapidjson/1.1.0@tencent/stable

[generators]
cmake

[imports]
include, * -> /opt/third/include
lib, * -> /opt/third/lib
```

## conan制作规范
### 名称结构
```
结构定义：<包名>/<版本号>@<所有者>/<成熟度>
成熟度：稳定度_库类型。稳定度有稳定版stable和开发版dev，库类型有静态库static和动态库无
示例：grpc/1.45.1@iBot/stable，grpc/1.45.1@iBot/stable_static
```

### 属性规范
| 项 | 发布方法 | 使用方法 |
| - | - | - |
| Windows/Linux | conan export-pkg的os | conan install的os |
| 32/64 | conan export-pkg的arch | conan install的arch |
| compiler.version | conan export-pkg的compiler.version | conan install的compiler.version |
| Debug/Release |  conanfile.py的self.cpp_info.release.libdirs | cmake的CMAKE_BUILD_TYPE |
| 动态库/静态库 | 都放到libdirs里，文件后缀区分 | 默认动态库，可指定具体文件名 |

```
conan install ../${productName}/conanfile.txt -s arch=x86_64 -s os=Linux -r cloud --update
conan install ..\\${productName}\\${conanfileTxtName} -s arch=${_CONAN_ARCH} -s os=Windows -s compiler.version=${compilerVersion} -r cloud --update

conan export-pkg ${conanfilePyName} ${_CONAN_NAME} -s arch=${arch} -s os=Windows -f
conan upload ${_CONAN_NAME} -r cloud --force —all
```

### 构建发布文件存放规则
1. 文件清单有：conanfile.py、构建脚本(Windows/Linux)、发布脚本(Windows/Linux)。
1. 外部库的文件存放在通用构建仓库，业务类(比如通用库)的文件存放在业务仓库(通用库仓库)的目录cicd/conan/

### 发布和脚本
#### 目录结构
| 目录 | 定义 | 说明 |
| - | - | - |
| include | 头文件 | conan指定的include目录，内有一层目录是库名称 |
| bin | Windows动态库时的dll目录，Windows静态库时的dll目录 | 有目录debug和release |
| lib | Windows静态库时的lib目录，Linux的so/a目录 | 有目录debug和release |

#### 常用语句
```
if self.settings.os == "Windows":
if self.settings.compiler == "Visual Studio":
```

#### conanfile.py文件
```
from conans import ConanFile, tools
import shutil
import os
import fnmatch

class TestConan(ConanFile):
    name = "test"
    version = "0.0.0"
    settings = "os", "compiler", "build_type", "arch"
    description = "conan template test file."
    url = "None"
    license = "None"
    author = "None"
    topics = None

    def package(self):
        self.copy("*")

    def package_info(self):
        self.cpp_info.includedirs = ["include"]
        self.cpp_info.release.libdirs = ["lib/release"]
        self.cpp_info.debug.libdirs = ['lib/debug']
        self.cpp_info.realse.bindirs = ["bin/release"]
        self.cpp_info.debug.bindirs = ["bin/debug"]
        if self.settings.os == "Windows":
            if os.path.exists('lib/release'):
                self.cpp_info.release.libs = fnmatch.filter(os.listdir('lib/release'), '*.lib')
            if os.path.exists('lib/debug'):
                self.cpp_info.debug.libs = fnmatch.filter(os.listdir('lib/debug'), '*.lib')
            if os.path.exists('bin/release'):
                self.cpp_info.release.bins = fnmatch.filter(os.listdir("bin/release"), '*')
            if os.path.exists('bin/debug'):
                self.cpp_info.debug.bins = fnmatch.filter(os.listdir("bin/debug"), '*')
        else:
            if os.path.exists('lib/release'):
                self.cpp_info.release.libs = fnmatch.filter(os.listdir('lib/release'), '*.a')
            if os.path.exists('lib/debug'):
                self.cpp_info.debug.libs = fnmatch.filter(os.listdir('lib/debug'), '*.so*')
```

## 资料
* [架构与术语](https://zhuanlan.zhihu.com/p/360348196)
* [conan一年使用总结](http://zhongpan.tech/2020/01/11/022-one-year-usage-summary-of-conan/)
* Conan公共仓库：https://conan.io/center/

### 打包
* [打包只有头文件的库](https://www.cnblogs.com/xl2432/p/11901089.html), https://docs.conan.io/en/latest/howtos/header_only.html?highlight=header%20only
* [conan打包脚本](https://chromium.googlesource.com/external/github.com/google/flatbuffers/+/c0698cc33f1e534bb59c455909b88cc2726089af/conanfile.py)
