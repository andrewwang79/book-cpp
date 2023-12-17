# 开发语法
## 常用
* [字符串和值转换](https://www.programiz.com/cpp-programming/string-float-conversion)
```
字符串到值：float num_float = std::stof(str);
char*到值：int val = atoi(char);
值到字符串：string s = std::to_string(intVal);
```

* [map](https://blog.csdn.net/sevenjoin/article/details/81943864)
```
std::map<std::string, ClassAbc> abcMap;
abcMap.insert("key", { "classProperty1", "classProperty2" });
```

* 变量声明时的赋值
```
ClassX {ClassA a; std::map<int, int> b;}
ClassA a, b;
std::vector<ClassX> classXs{{a, {{1, 1}, {2, 2}}}, {b, {{1, 5}}}};
```

* const

```
const uint8_t* pt：指向的数据是常量，指针本身的值可以修改
uint8_t* const pt：指针本身是常量，指向的数据可以修改
const uint8_t* const pt：指针本身和指向的数据都是常量，都不可修改

常量成员函数不会修改类的成员变量，用于类的常量实例
class MyClass {
    void printMessage() const { }
};
const MyClass obj;
obj.printMessage();
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

* pair和tuple

```
2个用pair，超过用tuple

std::vector<std::pair<A, B>> vec;
vec.emplace_back(a1, b1);
for (const auto& pair : vec) {
    sum += pair.first.getValue() + pair.second.getValue();
}

std::vector<std::tuple<A, B, C>> vec;
vec.emplace_back(a1, b1, c1);
for (const auto& tuple : vec) {
    sum += std::get<0>(tuple).getValue() + std::get<1>(tuple).getValue() + std::get<2>(tuple).getValue();
}
```

* [C++17中的std::any](https://hedzr.com/c++/variant/any-in-c++17/)
```
int value = std::any_cast<int>(a);
```

* misc
```
std::this_thread::sleep_for(std::chrono::seconds(10));
```

## 智能指针
### [使用](https://javajgs.com/archives/114676)
```
p = new int(1024);//错误：不能将一个指针赋予shared_ptr
p.reset(new int(1024));//正确。p指向一个新对象

shared_ptr<int> p1 = new int(1024);//错误：必须使用直接初始化形式
shared_ptr<int> p2(new int(1024));//正确：使用了直接初始化形式

// 标准用法
shared_ptr<Class> ptrClass(new Class()); // 尽量不要用
shared_ptr<Class> ptrClass = make_shared<Class>();

// 有构造函数的模板类
shared_ptr<模板类<类型参数类>> ptrClass(new 模板类<类型参数类>(构造函数的参数));
shared_ptr<模板类<类型参数类>> ptrClass = make_shared<模板类<类型参数类>>(构造函数的参数);

// 智能指针继承关系类向下转换
shared_ptr<Class> ptrDynamicClass = make_shared<ChildClass>();
shared_ptr<ChildClass> ptrChildClass = dynamic_pointer_cast<ChildClass>(ptrDynamicClass);

// unique_ptr
unique_ptr<Class> u1 = make_unique<Class>();
auto u2 = u1; // unique_ptr不支持普通的拷贝或赋值操作。编译报错，需要用下面的move
auto u2 = std::move(u1); // u2获得内存所有权，u1此时等于nullptr

// shared_from_this
std::enable_shared_from_this从类成员函数中获取1个新的std::shared_ptr(类对象需用shared_ptr创建)，而不是this指针。确保对象在成员函数里执行异步操作时自身不会被意外销毁。
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

## 类成员变量及其初始化
| 类型 | 定义方法 | 初始化位置 | 使用场景 |
| - | - | - | - |
| 标准 | ClassA a_ | 构造函数的初始化列表 | 在ClassX构造函数里创建的 |
| 引用 | ClassB& b_ | 构造函数的初始化列表 | 推荐只用于单例或父子类的子级，因为对象生命周期管理不可控 |
| 智能指针 | std::unique_ptr<ClassC> c_ | 任意位置 | 无法在ClassX构造函数里创建的，比如构造需要的参数在ClassX::init()里 |

1. 构造函数的初始化列表 : 成员变量定义顺序和初始化列表顺序要一致，执行顺序是定义顺序
1. 类型类的构造函数执行：定义时不会，初始化列表时会
```
class ClassX {
public:
    ClassX(int value, ClassB& b) : a_(value), b_(b) { 初始化列表时会调用构造函数
        a_ = ClassA(value); // 赋值时会调用构造函数(a_第二次调用构造函数)
    }
    init(int value) {
      c_ = std::make_unique<ClassC>(value);
    }
private:
    ClassA a_;
    ClassB& b_;
    std::unique_ptr<ClassC> c_;
};
```

## 宏定义
* 宏定义不受命名空间的影响
* [宏定义的#和##](https://blog.csdn.net/qq_27074387/article/details/51646681)
* [操作系统的宏定义](https://blog.51cto.com/u_8081755/3351682), _WIN32包括Windows的32位和64位
* 宏定义使用

```
操作系统
#ifdef _WIN32
  //define something for Windows (32-bit and 64-bit, this part is common)
#elif __linux__
  //define something for Windows (64-bit only)
#else
#endif

编译方式
#ifdef DEBUG
#endif
```

## 指令
```
#warning "abc" // 编译警告
#error "xyz" // 编译错误，强制退出编译过程
```

## enum
* [enum与int转换](https://blog.csdn.net/Dream_Weave/article/details/83411570)

## 异常
```
#include <exception>
try {
} catch (std::exception& ex) {}
} catch (...) {}
```

## Lambda函数
> C++11中引入的一种匿名函数，它可以在需要函数对象的任何地方使用，并且可以捕获上下文中的变量。

```
int x = 10;
auto add_x = [x](int a) -> int { return a + x; };
[x]是Lambda函数外的上下文变量x，=是类实例本身；(int a)表示输入参数是int；-> int表示返回一个整数；{ }表示实现函数体。
```

## 类型转换
* [C++类型转换：隐式类型转换、类类型转换、显示类型转换](https://segmentfault.com/a/1190000016582440)

| 项 | 说明 | 实例 |
| - | - | - |
| 隐式类型转换 | 最宽的数据类型成为目标转换类型 | double difference(int ival1, int ival2) { return ival1 - ival2; // 返回值被提升为double类型 } |
| 显式类型转换-C | (type-id) | double x = (double)intVal; |
| 显式类型转换-C++ | static_cast、dynamic_cast、reinterpret_cast、const_cast | / |

> 显式类型转换-C++

| 项 | 定义 | 转换源 | 说明 | 实例 |
| - | - | - | - | - |
| static_cast | 静态类型转换 | 任意数据 | 不安全，void*转换 | double y = static_cast<double>(x); |
| dynamic_cast | 动态类型转换 | 继承类 | 确保是继承关系 | DerivedClass* BaseClass* basePtr = new DerivedClass(); derivedPtr = dynamic_cast<DerivedClass*>(basePtr); |
| reinterpret_cast | 重新解释的转换 | 任意数据 | 危险 | / |
| const_cast | 去除常量属性的转换 | 常量 | 危险 | const int x = 10; int* nonConstPtr = const_cast<int*>(&x); |

## 知识
* [function](https://blog.csdn.net/weixin_43712770/article/details/120738647)
* [模板类和模板函数](http://c.biancheng.net/view/320.html)
* [模板别名](https://wizardforcel.gitbooks.io/cpp-11-faq/content/55.html)
* [右值引用](https://changkun.de/modern-cpp/zh-cn/03-runtime/index.html#3-3-%E5%8F%B3%E5%80%BC%E5%BC%95%E7%94%A8)
* [explicit构造函数](https://www.cnblogs.com/likebeta/archive/2012/07/31/explicit.html), 所有单参数的构造函数都必须是显示的
