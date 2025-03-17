# 开发
## 多线程
* QFuture和QtConcurrent是高层次的并发编程接口。适用于需要快速并行执行任务，不需要直接管理线程。
* QThread和QThreadPool是细粒度控制线程。moveToThread()将一个对象的某些操作移动到另一个线程中执行。
* 在 Qt 中，所有的GUI操作必须在主线程中进行。如果你在非主线程中调用QMessageBox::information或其他任何涉及GUI的操作，可能会导致应用程序崩溃或行为异常。这是因为Qt的GUI库不是线程安全的。

## 资源变动
* 资源文件有变动：C:\Qt\5.12.9\msvc2017_64\bin\rcc.exe -binary Resource.qrc -o Resource.rcc
* ts文件有变动
  1. 生成qm文件
      * Qt命令：lrelease file.ts
      * Qt工具"Linguist"：打开ts文件; 文件 -> 发布翻译(Release)
  1. 执行“资源文件有改动”

## 数据库SQLite
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