# C++
* [最佳实践代码](https://github.com/andrewwang79/cpp.practice.git)

## 现代C++
* [C++ 参考手册](https://zh.cppreference.com/)
* [C++11\14\17\20 特性介绍](https://www.jianshu.com/p/8c4952e9edec)
* [现代C++教程](https://changkun.de/modern-cpp/zh-cn/00-preface/)

## 划重点
* [右值引用](https://changkun.de/modern-cpp/zh-cn/03-runtime/index.html#3-3-%E5%8F%B3%E5%80%BC%E5%BC%95%E7%94%A8)
* [weak_ptr](https://blog.csdn.net/c_base_jin/article/details/79440999)：不计数的弱引用

### RTTI（Run-Time Type Identification)
* 获取类名，类构造函数时建立类名和类实例的关系，关系注册到全局静态实例。实现类似Java的反射
* https://blog.csdn.net/gatieme/article/details/50947821
* https://blog.csdn.net/TH_NUM/article/details/86570618

```
  typeid(ABC::TypeClass).name() // N3ABC9TypeClassE
  abi::__cxa_demangle(typeid(ABC::TypeClass).name(), nullptr, nullptr, nullptr) // ABC::TypeClass，适用linux
```
