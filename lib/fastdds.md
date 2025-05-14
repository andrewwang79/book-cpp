# FastDDS
* [eProsima Fast DDS](https://github.com/eProsima/Fast-DDS)

## 安装
* [安装手册](https://eprosima-dds-router.readthedocs.io/en/latest/rst/developer_manual/installation/sources/linux.html)

## 资料
* hello_world实例 : [代码下载](https://github.com/eProsima/Fast-DDS/tree/master/examples/cpp/hello_world)，[参考资料](https://blog.csdn.net/weixin_38184628/article/details/144150300)

## FastDDSGen
### 安装
* [Linux安装手册](https://fast-dds.docs.eprosima.com/en/latest/installation/sources/sources_linux.html#fast-dds-gen-installation)
* 下载安装[java](https://www.openlogic.com/openjdk-downloads?field_java_parent_version_target_id=807&field_operating_system_target_id=436&field_architecture_target_id=391&field_java_package_target_id=396)
* FastDDSGen.jar生成
```
git clone --recursive -b v4.0.4 https://github.com/eProsima/Fast-DDS-Gen.git fastddsgen
cd fastddsgen
gradlew assemble

创建文件fastddsgen.bat
@echo off
java -jar "C:\WINDOWS\fastddsgen.jar" %*

生成的fastddsgen.jar和bat放到%path%的目录下，当前用C:\WINDOWS\
```

### 使用
```
java -jar c:/windows/fastddsgen.jar -replace HelloWorld.idl
fastddsgen HelloWorld.idl
```