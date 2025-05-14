# vcpkg
> C++包管理系统

## Windows安装
1. 下载[vcpkg](https://github.com/microsoft/vcpkg/releases/tag/2025.04.09)到C:\vcpkg\
1. 运行bootstrap-vcpkg.bat
1. ```setx VCPKG_ROOT "C:\vcpkg\"```

### 常用命令
```
vcpkg install fastdds
vcpkg list
set path="%path%;C:\vcpkg\installed\x64-windows\debug\bin"
```

### 注意事项
* install时，下载github等慢就手工下载并放到目录，重新执行install：C:\vcpkg\downloads\