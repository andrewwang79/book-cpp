# vcpkg
> C++包管理系统

## 安装
1. 下载[vcpkg](https://github.com/microsoft/vcpkg/releases/tag/2025.04.09)

### Windows
1. 下载包解压到C:\vcpkg\
1. 运行bootstrap-vcpkg.bat
1. 环境变量"PATH"追加C:\vcpkg\
1. ```setx VCPKG_ROOT C:\vcpkg\```

### Linux/Mac
1. 下载包解压到/opt/vcpkg/
1. 运行bootstrap-vcpkg.sh

## 常用命令
```
vcpkg install fastdds
vcpkg list
```

## 注意事项
* install时下载github源慢可以手工下载并放到vcpkg的下载目录(如Windows的C:\vcpkg\downloads\)，重新执行install
* install库的路径设置到环境变量：set PATH="%PATH%;C:\vcpkg\installed\x64-windows\debug\bin"
* cmake编译：```cmake -S . -B build -DCMAKE_TOOLCHAIN_FILE=/opt/vcpkg/scripts/buildsystems/vcpkg.cmake```