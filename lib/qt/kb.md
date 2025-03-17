# QT
## 知识
* [教程1](https://blog.csdn.net/tiao_god/category_9462012.html)
* [窗口部件类图](https://blog.csdn.net/kilotwo/article/details/79238545)

## 安装
* [Qt安装教程](http://c.biancheng.net/view/3858.html)
* 国内镜像地址

|host| link |
|-|-|
|中国科学技术大学| http://mirrors.ustc.edu.cn/qtproject/ |
|清华大学| https://mirrors.tuna.tsinghua.edu.cn/qt/ |
|北京理工大学| http://mirror.bit.edu.cn/qtproject/ |
|中国互联网络信息中心| https://mirrors.cnnic.cn/qt/|

### Qt + VisualStudio2017
1. 安装VisualStudio2017：安装时需选择“使用C++的桌面开发”，勾选右边的”MSVC 2017”和“Windows10SDK”
1. QT下载和安装(版本5.12.9)：[在线安装工具](https://download.qt.io/archive/online_installers/4.4/)，[历史版本（以5.12.9为例）](https://download.qt.io/archive/qt/5.12/5.12.9/)
1. 安装VS插件：安装好后配置QT的安装路径。
  * 在线安装：在VS2017的"扩展和更新-联机"中搜索插件"Qt Visual Studio Tools"
  * 离线安装：下载[插件](https://download.qt.io/development_releases/vsaddin)，推荐版本2.9.0

### Qt5 + Linux
* Linux如果不是桌面版，可以在Windows使用远程终端软件/vscode等打开Qt程序实现开发和调试(XWindows机制), [远程开发VsCode终端中运行PyQt5程序](https://blog.csdn.net/qq_41092406/article/details/118201187)
* [安装说明](https://wiki.qt.io/Install_Qt_5_on_Ubuntu)
```
wget https://download.qt.io/archive/qt/5.12/5.12.9/qt-opensource-linux-x64-5.12.9.run
chmod +x qt-opensource-linux-x64-5.12.9.run
./qt-opensource-linux-x64-5.12.9.run # 只能安装到默认路径/opt/Qt5.12.9，选择其他目录会有潜在风险
选择所需的qt模块，如X64的
```

### VSCode
* 在VsCode中按F1打开命令行, 输入QtConfigure : Set Qt Dir 选择QT安装目录，注意 这里安装目录指的是5.12.9文件夹的上一层 如：C:/Qt
* 在VsCode中按F1打开命令行, 输入QtConfigure : Open Qt Designer 打开 Qt Designer，该命令仅在生成 Qt Ui项目后才能使用 
* 在VsCode中按F1打开命令行, 输入QtConfigure : Open Qt Assistant 打开 Qt Assistant
* Extensions: CMake, CMake Tools, Qt Configure, Qt tools
* [vsCode开发Qt项目配置分享](https://www.bilibili.com/video/av596737763/)

### 资料
* [VS2019 + Qt5.12 配置完成后，无法打开 Qt 源文件解决方案](https://blog.csdn.net/weixin_47156401/article/details/120626400)
