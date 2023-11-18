# boost
* 部分库是header-only，部分库必须编译

## 构建
* [linux上构建库](https://www.boost.org/doc/libs/1_78_0/more/getting_started/unix-variants.html)

## 集成
* [自动链库](https://blog.csdn.net/zyhse/article/details/105411430)
* header-only需加编译项：add_compile_definitions(BOOST_UUID_FORCE_AUTO_LINK BOOST_ALL_NO_LIB)

## 语法
```
boost::replace_all(path, "//", "/"); // 字符串替换
(boost::format("log info : path[%s], val[%d].") % path % val))
```

## 网络
* [网络库boost asio](https://zhuanlan.zhihu.com/p/634878993)

## 资料
* [C++使用Boost实现HTTP服务端——同步、异步、协程](https://blog.csdn.net/luchengtao11/article/details/100928141)
* [Boost Beast要点解析](https://blog.csdn.net/guxch/article/details/106780832)
