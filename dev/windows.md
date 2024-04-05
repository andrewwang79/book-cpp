# Windows开发

## 常用命令
* [查看dll或exe文件的依赖项和符号表](https://blog.csdn.net/weixin_44120025/article/details/118651103)
  * dumpbin /dependents dll/exe // 依赖库
  * dumpbin /exports dll // 符号表
  * dumpbin /symbols dll/exe // 感觉没啥用
* [查看程序运行时调用的动态链接库](https://blog.csdn.net/fightsyj/article/details/107043353) : 资源监视器 -> CPU标签页 -> 关联的模块

## 导入导出
* [__declspec(dllimport/dllexport)](https://www.jianshu.com/p/ea45468f25f1)
  * __declspec(dllexport)：导出符号，也就是定义该函数的dll
  * __declspec(dllimport)：导入符号，也就是使用该函数

```
// VS的“预编译”选项里定义_EXPORTING宏
#ifdef _EXPORTING
#define API_DECLSPEC __declspec(dllexport)
#else
#define API_DECLSPEC __declspec(dllimport)
#endif
​
API_DECLSPEC void HelloWorld();
class API_DECLSPEC XyzClass{};
```

## 库
* 编译链接时会lib文件里会记录其对应的dll，所以不能修改dll文件名

## Visual Studio
```
错误：该文件包含不能在当前代码页(936)中表示的字符。请将该文件保存为 Unicode 格式以防止数据丢失
原因：Windows的默认编码是GBK(936)，如果不指定UTF8则VisualStudio会用系统默认编码编译项目(GBK下UTF8文件里的中文就会乱)，所以项目要明确编码：
解决：右键点击项目 -> "属性(Properties)" -> "配置属性(Configuration Properties)" -> "C/C++" -> "命令行(Command Line)"
在右侧的 "附加选项(Additional Options)" 中，输入 "/utf-8"。
```