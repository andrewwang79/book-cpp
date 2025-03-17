# 插件
## 软件使用的插件机制
* 3DSlicer：支持2种主要类型的插件
        1. CLI（Command Line Interface）模块：用C++编写，创建一个新的目录作为插件的根目录。在该目录中创建必要的子目录和文件，比如CMakeLists.txt、源代码文件、配置文件等。
        1. 脚本化模块：通常用Python编写，直接集成到Slicer的用户界面中。
* meshlab：同CLI

## 第三方插件库
* ROS插件库：库pluginlib
* Boost插件库：Boost.Extension
* Qt插件
    * QPluginLoader类，插件必须实现特定的接口，通过宏Q_PLUGIN_METADATA和Q_EXPORT_PLUGIN2来注册
    * [CTK Plugin Framework](https://cloud.tencent.com/developer/article/1984142)