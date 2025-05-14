# FastDDS
* [eProsima Fast DDS](https://github.com/eProsima/Fast-DDS)，[产品下载](https://www.eprosima.com/product-download)

## 安装
* [安装手册](https://eprosima-dds-router.readthedocs.io/en/latest/rst/developer_manual/installation/sources/linux.html)
* [eProsima Fast DDS 3.2.2下载](https://www.eprosima.com/component/ars/items/eprosima-fast-dds-3-2-2)，有Windows安装包

### FastDDSGen
* DDS安装包内置FastDDSGen，以下是单独安装时使用。但Java要自行安装
#### Linux安装
* [Linux安装手册](https://fast-dds.docs.eprosima.com/en/latest/installation/sources/sources_linux.html#fast-dds-gen-installation)

#### Windows安装
1. 安装[Java](https://www.openlogic.com/openjdk-downloads?field_java_parent_version_target_id=807&field_operating_system_target_id=436&field_architecture_target_id=391&field_java_package_target_id=396)
1. 生成FastDDSGen.jar
    ```
    git clone --recursive -b v4.0.4 https://github.com/eProsima/Fast-DDS-Gen.git fastddsgen
    cd fastddsgen
    gradlew assemble
    ```
1. 安装FastDDSGen.jar
    ```
    生成的fastddsgen.jar和fastddsgen.bat放到%path%的目录下，当前用C:\WINDOWS\
    fastddsgen.bat文件内容：
    @echo off
    java -jar "C:\WINDOWS\fastddsgen.jar" %*
    ```

#### 使用
```
java -jar c:/windows/fastddsgen.jar -replace HelloWorld.idl
fastddsgen HelloWorld.idl
```

## 资料
* hello_world实例 : [代码下载](https://github.com/eProsima/Fast-DDS/tree/master/examples/cpp/hello_world)，[参考资料](https://blog.csdn.net/weixin_38184628/article/details/144150300)

## 配套工具
* eProsima Shapes Demo：图形化应用程序展示 eProsima Fast RTPS 的能力。用户可通过该程序直观理解DDS的概念，如发布者、订阅者、主题等。可用于实时数据可视化、多主题管理等场景。
* eProsima Fast DDS Monitor：图形化桌面应用程序监控使用 eProsima Fast DDS 库部署的DDS环境。用户可实时跟踪DDS实体之间发布 / 订阅通信的状态，选择多种通信参数进行测量，并对这些参数进行记录和统计计算，还能快速检测DDS环境网络中可能出现的问题或异常事件。