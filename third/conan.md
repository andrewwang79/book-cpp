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

## conanfile.txt编写
```
[requires]
rapidjson/1.1.0@tencent/stable

[generators]
cmake

[imports]
include, * -> /opt/third/include
lib, * -> /opt/third/lib
```

## conanfile.py编写
```
from conans import ConanFile, tools
import shutil
import fnmatch
import os

if self.settings.os == "Windows":
if self.settings.compiler == "Visual Studio":

def package(self):
  self.copy(pattern="LICENSE.txt", dst="licenses")
  self.copy(pattern="*.h", src="Abc", dst="inc", keep_path=True)
  self.copy(pattern="AbcD.*", src="lib", dst="lib/debug", keep_path=True)
  self.copy(pattern="Abc.*", src="lib", dst="lib/release", keep_path=True)

def package_info(self):
    self.cpp_info.includedirs = ["inc"]
    self.cpp_info.debug.libdirs = ['lib/debug']
    self.cpp_info.release.libdirs = ["lib/release"]
    self.cpp_info.debug.libs = fnmatch.filter(os.listdir('lib/debug'), '*')
    self.cpp_info.release.libs = fnmatch.filter(os.listdir('lib/release'), '*')
```

## 资料
* 结构定义：<包名>/<版本号>@<所有者>/<成熟度>，如rapidjson/1.1.0@tencent/stable
* [架构与术语](https://zhuanlan.zhihu.com/p/360348196)
* [conan一年使用总结](http://zhongpan.tech/2020/01/11/022-one-year-usage-summary-of-conan/)
* Conan公共仓库：https://conan.io/center/

### 打包
* [打包只有头文件的库](https://www.cnblogs.com/xl2432/p/11901089.html), https://docs.conan.io/en/latest/howtos/header_only.html?highlight=header%20only
* [conan打包脚本](https://chromium.googlesource.com/external/github.com/google/flatbuffers/+/c0698cc33f1e534bb59c455909b88cc2726089af/conanfile.py)
