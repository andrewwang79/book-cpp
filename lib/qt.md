# QT
## 常用语句
```
// char*转换到string
char* buffer = new char[512];
gdcmIO->GetPatientSex(buffer);
std::string str;
str.assign(buffer, std::strlen(buffer));
delete[] buffer;

// QString转换到string
QString qstr = "Hello, world!";
std::string str = qstr.toStdString();

// string转换到QString
std::string str = "Hello, world!";
QString qstr = QString::fromStdString(str);

// QString和数字互转
int num;
QString str = QString::number(num);

// QString
QString("Select id from user where account = '%1' and password = '%2'").arg(userName, password); // 格式化
QString s;
isNull情况: s.clear(), s = nullptr
isEmpty情况：(s = nullptr)以外的都符合情况

// log
qDebug() << "name: " << name;

// SQLite的时间
SQL : create_time TEXT NOT NULL DEFAULT (datetime('now', 'localtime'))
获取当前时间 : QDateTime::currentDateTime();
// SQLite的执行。bindValue会自动适配数据类型(如字符串或int)，不需要因为字符串而在SQL上加''(如加':account')
QSqlQuery sq;
sq.prepare("SELECT id FROM user WHERE account = :account");
sq.bindValue(":account", value);

// 子线程执行
QFuture<bool> tFuture = QtConcurrent::run([=]() -> bool {
    return true;
});
while (!tFuture.isFinished()) {
  QApplication::processEvents(QEventLoop::AllEvents, 500);  //唤醒主界面
}
bool success = tFuture.result();
```

## 知识
* [教程1](https://blog.csdn.net/tiao_god/category_9462012.html)
* [窗口部件类图](https://blog.csdn.net/kilotwo/article/details/79238545)

### SQLite
* [操作SQLite数据库](https://zhuanlan.zhihu.com/p/615519914)
* [QSqlQuery、QSqlQueryModel、QSqlTableModel](https://blog.csdn.net/aggie4628/article/details/102770029)
* 管理软件：DB.Browser.for.SQLite-3.12.2-win64

```
生成删除所有表的SQL：
SELECT 'DROP TABLE IF EXISTS "' || name || '";' FROM sqlite_master WHERE type = 'table'
SQL构建：字符串和数字：
QString("SELECT '%1'").arg("name");
QString("SELECT %1").arg(QString::number(22);
```

### 多线程
* QFuture和QtConcurrent是高层次的并发编程接口。适用于需要快速并行执行任务，不需要直接管理线程。
* QThread和QThreadPool是细粒度控制线程。moveToThread()将一个对象的某些操作移动到另一个线程中执行。
* 在 Qt 中，所有的GUI操作必须在主线程中进行。如果你在非主线程中调用QMessageBox::information或其他任何涉及GUI的操作，可能会导致应用程序崩溃或行为异常。这是因为Qt的GUI库不是线程安全的。

### 消息机制
#### 基于对象的线程模型
* 将每个对象绑定到一个特定的线程(对象创建时所在的线程)，每个线程有事件循环
* 对象会使用其绑定线程的事件循环处理，而不是当前运行中的线程。在接收方对象所属的线程执行槽函数，确保了线程安全、对象状态一致性和简化编程模型。
* QObject::moveToThread可以改变对象绑定的线程
* 通过智能指针传递大数据，否则自定义结构会深拷贝
* QObject::deleteLater是QObject 类的一个成员函数。当你调用这个函数时，它会将一个事件排入事件循环，请求在控制权返回事件循环时删除对象。这个特性在跨线程操作时特别有用，因为它能确保对象被安全地删除，而且是在适当的线程上。

> 两种机制：全局信号槽机制，事件过滤器

#### emit
1. 跨窗口或者后台服务的消息通知用emit
1. Qt会在信号接收对象所在的线程中调用槽函数。如果信号和接收对象在同一线程中，则槽函数将直接在该线程中执行。如果信号和接收对象不在同一线程中，则Qt会将槽函数的执行请求发送到接收对象所在的线程，并在那里执行槽函数。

### 资源变动
* 资源文件有变动：C:\Qt\5.12.9\msvc2017_64\bin\rcc.exe -binary Resource.qrc -o Resource.rcc
* ts文件有变动
  1. 生成qm文件
      * Qt命令：lrelease file.ts
      * Qt工具"Linguist"：打开ts文件; 文件 -> 发布翻译(Release)
  1. 执行“资源文件有改动”

### QT通讯
1. 在组件内部定义connect ： 如果信号和槽的连接逻辑紧密相关于组件的内部行为，可以在组件内部定义 connect。这种方式有助于组件的封装性和自包含性。
1. 在窗口定义connect：如果信号和槽的连接涉及多个组件的交互，通常在窗口中定义 connect。这样可以集中管理组件之间的交互逻辑，保持组件的独立性和重用性。
1. 在专门的管理对象定义connect：如中介者(Controller)或事件总线（Event Bus）。前者一般1对1一个窗口，后者是系统级的（signal有分类区分不同的窗口）

## 安装
### Qt + VisualStudio2017
1. 安装VisualStudio2017：安装时需选择“使用C++的桌面开发”，勾选右边的”MSVC 2017”和“Windows10SDK”
1. 安装QT：[下载](https://download.qt.io/archive/online_installers/4.4/)
1. 安装VS插件：安装好后配置QT的安装路径。
  * 在线安装：在VS2017的"扩展和更新-联机"中搜索插件"Qt Visual Studio Tools"
  * 离线安装：下载[插件](https://download.qt.io/development_releases/vsaddin)，推荐版本2.9.0

### Qt5 + Linux
* Linux如果不是桌面版，可以在Windows使用远程终端软件/vscode等打开Qt程序实现开发和调试(XWindows机制), [远程开发VsCode终端中运行PyQt5程序](https://blog.csdn.net/qq_41092406/article/details/118201187)
* [安装说明](https://wiki.qt.io/Install_Qt_5_on_Ubuntu)
```
wget https://download.qt.io/archive/qt/5.12/5.12.9/qt-opensource-linux-x64-5.12.9.run
chmod +x qt-opensource-linux-x64-5.12.9.run
./qt-opensource-linux-x64-5.12.9.run # 只能安装到默认路径/opt/Qt5.12.9，选择其他目录会有潜在风险
选择所需的qt模块，如X64的
```

### 资料
* [VS2019 + Qt5.12 配置完成后，无法打开 Qt 源文件解决方案](https://blog.csdn.net/weixin_47156401/article/details/120626400)
