# 语法
## 常用
```
// shared_ptr, 尽量不要用shared_ptr<Class> ptrClass(new Class());
shared_ptr<Class> ptrClass = make_shared<Class>();
shared_ptr<Class> ptrDynamicClass = make_shared<ChildClass>();
shared_ptr<ChildClass> ptrChildClass = dynamic_pointer_cast<ChildClass>(ptrDynamicClass); // 智能指针继承关系类向下转换

// unique_ptr
unique_ptr<Class> u1 = make_unique<Class>();
auto u2 = u1; // 编译报错
auto u2 = std::move(u1); // u2获得内存所有权，u1此时等于nullptr

// [字符串和值转换](https://www.programiz.com/cpp-programming/string-float-conversion)
字符串到值：float num_float = std::stof(str);
值到字符串：string s = std::to_string(intVal);
```

## 重点知识
* [C++类型转换：隐式类型转换、类类型转换、显示类型转换](https://segmentfault.com/a/1190000016582440)
* [map](https://blog.csdn.net/sevenjoin/article/details/81943864)
* [function](https://blog.csdn.net/weixin_43712770/article/details/120738647)
* [模板类和模板函数](http://c.biancheng.net/view/320.html)
* [模板别名](https://wizardforcel.gitbooks.io/cpp-11-faq/content/55.html)
* [C++17中的std::any](https://hedzr.com/c++/variant/any-in-c++17/)
* [右值引用](https://changkun.de/modern-cpp/zh-cn/03-runtime/index.html#3-3-%E5%8F%B3%E5%80%BC%E5%BC%95%E7%94%A8)

### 智能指针
* [智能指针的正确使用方式](https://cloud.tencent.com/developer/article/1517336)
* [unique_ptr](https://blog.csdn.net/afei__/article/details/80670283)
* [weak_ptr](https://blog.csdn.net/c_base_jin/article/details/79440999)：不计数的弱引用
