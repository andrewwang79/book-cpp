# Windows开发

## __declspec(dllimport/dllexport)
* https://www.jianshu.com/p/ea45468f25f1
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
