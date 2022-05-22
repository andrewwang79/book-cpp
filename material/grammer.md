# 语法
## 常用
```
shared_ptr<Class> ptrClass = make_shared<Class>();
shared_ptr<Class> ptrClass(new Class());
shared_ptr<ChildClass> ptrChildClass = dynamic_pointer_cast<ChildClass>(ptrClass);
```

## 重点知识
* [C++类型转换：隐式类型转换、类类型转换、显示类型转换](https://segmentfault.com/a/1190000016582440)
* [map](https://blog.csdn.net/sevenjoin/article/details/81943864)
* [function](https://blog.csdn.net/weixin_43712770/article/details/120738647)
* [模板别名](https://wizardforcel.gitbooks.io/cpp-11-faq/content/55.html)
* [C++17中的std::any](https://hedzr.com/c++/variant/any-in-c++17/)
* [右值引用](https://changkun.de/modern-cpp/zh-cn/03-runtime/index.html#3-3-%E5%8F%B3%E5%80%BC%E5%BC%95%E7%94%A8)
* [weak_ptr](https://blog.csdn.net/c_base_jin/article/details/79440999)：不计数的弱引用
