# 语法
## 常用
* [字符串和值转换](https://www.programiz.com/cpp-programming/string-float-conversion)
```
字符串到值：float num_float = std::stof(str);
值到字符串：string s = std::to_string(intVal);
```
* [文件名和文件行](https://blog.csdn.net/nyist_zxp/article/details/107890791)
* 静态实例
```
class ClassAbc {
public:
  static ClassAbc& getInstance() {
    static ClassAbc instance;
    return instance;
  }
}
```

## 智能指针
### [使用](https://javajgs.com/archives/114676)
```
p = new int(1024);//错误：不能将一个指针赋予shared_ptr
p.reset(new int(1024));//正确。p指向一个新对象

shared_ptr<int> p1 = new int(1024);//错误：必须使用直接初始化形式
shared_ptr<int> p2(new int(1024));//正确：使用了直接初始化形式

// shared_ptr, 尽量不要用shared_ptr<Class> ptrClass(new Class());
shared_ptr<Class> ptrClass = make_shared<Class>();
shared_ptr<Class> ptrDynamicClass = make_shared<ChildClass>();
shared_ptr<ChildClass> ptrChildClass = dynamic_pointer_cast<ChildClass>(ptrDynamicClass); // 智能指针继承关系类向下转换

// unique_ptr
unique_ptr<Class> u1 = make_unique<Class>();
auto u2 = u1; // unique_ptr不支持普通的拷贝或赋值操作。编译报错，需要用下面的move
auto u2 = std::move(u1); // u2获得内存所有权，u1此时等于nullptr
```

### 知识
* [智能指针的正确使用方式](https://cloud.tencent.com/developer/article/1517336)
* [C++之动态内存以及智能指针](https://blog.csdn.net/weixin_43518637/article/details/110942288)
* [智能指针shared_ptr、unique_ptr、weak_ptr](https://blog.csdn.net/wanggao_1990/article/details/97932162)
* [unique_ptr](https://blog.csdn.net/afei__/article/details/80670283), [从C++20 shared_ptr移除unique()方法浅析多线程同步](https://blog.csdn.net/zxpoiu/article/details/111255182)
* 当一个对象的析构非常耗时，甚至影响到了关键线程的速度。可以使用BlockingQueue<std::shared_ptr<void>> 将对象转移到另外一个线程中释放，从而解放关键线程。

#### shared_ptr
* 计数加1：用一个shared_ptr初始化另一个shared_ptr；函数参数；函数返回值
* 计数减1：赋值一个新值 (operator= 或 reset() 赋值)；离开作用域
* 支持异常时安全delete
#### weak_ptr
* 不计数的弱引用，解决shared_ptr双向引用的问题
* [weak_ptr](https://blog.csdn.net/c_base_jin/article/details/79440999)

## map
* [map](https://blog.csdn.net/sevenjoin/article/details/81943864)
```
std::map<std::string, ClassAbc> abcMap;
abcMap.insert("key", { "classProperty1", "classProperty2" });
```

## 操作系统的宏定义
* [操作系统的宏定义](https://blog.51cto.com/u_8081755/3351682), _WIN32包括Windows的32位和64位
* [宏定义的#和##](https://blog.csdn.net/qq_27074387/article/details/51646681)
* 宏定义使用
```
#ifdef _WIN32
  //define something for Windows (32-bit and 64-bit, this part is common)
#elif __linux__
  //define something for Windows (64-bit only)
#else
#endif
```

## enum
* [enum与int转换](https://blog.csdn.net/Dream_Weave/article/details/83411570)

## 知识
* [C++类型转换：隐式类型转换、类类型转换、显示类型转换](https://segmentfault.com/a/1190000016582440)
* [function](https://blog.csdn.net/weixin_43712770/article/details/120738647)
* [模板类和模板函数](http://c.biancheng.net/view/320.html)
* [模板别名](https://wizardforcel.gitbooks.io/cpp-11-faq/content/55.html)
* [C++17中的std::any](https://hedzr.com/c++/variant/any-in-c++17/)
* [右值引用](https://changkun.de/modern-cpp/zh-cn/03-runtime/index.html#3-3-%E5%8F%B3%E5%80%BC%E5%BC%95%E7%94%A8)
* [explicit构造函数](https://www.cnblogs.com/likebeta/archive/2012/07/31/explicit.html), 所有单参数的构造函数都必须是显示的
