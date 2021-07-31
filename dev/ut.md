# UT
* 如果需要单元测试私有函数，说明代码需要重构

## gtest
* gtest编译同样的源代码和UT，生成文件的签名是不一样，应该是加了随机数
* https://google.github.io/googletest/
* [c++和 gmock环境搭建方法](https://my.oschina.net/donngchao/blog/4884051)
* [玩转Google开源C++单元测试框架Google Test系列(gtest)(总)](https://www.cnblogs.com/coderzh/archive/2009/04/06/1426755.html)
* [demo](https://github.com/andrewwang79/cpp.practice/tree/master/gtest)

### mock
* https://zhuanlan.zhihu.com/p/101906555

## boost test
* Boost：https://www.jianshu.com/p/9a87918023fb
* [指定case测试](https://www.boost.org/doc/libs/1_47_0/libs/test/doc/html/utf/user-guide/runtime-config/run-by-name.html)：./ut_exe --run_test=Suite/Case

## UT覆盖率报告lcov
* [下载](http://ltp.sourceforge.net/coverage/lcov.php)
```
wget -O lcov-1.14.tar.gz http://downloads.sourceforge.net/ltp/lcov-1.14.tar.gz
tar zxvf lcov-1.14.tar.gz && cd lcov-1.14
make install
```
* [使用，可以去除不要的目录](https://blog.csdn.net/weixin_43892514/article/details/107822769)

## 资料
* [在 Visual Studio 中编写 C/C++ 单元测试](https://docs.microsoft.com/zh-cn/visualstudio/test/writing-unit-tests-for-c-cpp?view=vs-2019)
* [c++编译时如何把private属性变成public？](https://blog.csdn.net/qq910894904/article/details/38583593)
