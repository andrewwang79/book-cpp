# QT
## 使用
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


// QString
QString("Select id from user where account = '%1' and password = '%2'").arg(userName, password); // 格式化
QString s;
isNull情况: s.clear(), s = nullptr
isEmpty情况：(s = nullptr)以外的都符合情况
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

### 图像处理规则
1. 都采用世界坐标
1. ITK和VTK和世界坐标可转换

## 安装
1. 安装VisualStudio2017：安装时需选择“使用C++的桌面开发”，勾选右边的”MSVC 2017”和“Windows10SDK”
1. 安装QT：[下载](https://download.qt.io/archive/online_installers/4.4/)
1. 安装VS插件：安装好后配置QT的安装路径。
  * 在线安装：在VS2017的"扩展和更新-联机"中搜索插件"Qt Visual Studio Tools"
  * 离线安装：下载[插件](https://download.qt.io/development_releases/vsaddin)，推荐版本2.9.0

### 资料
* [VS2019 + Qt5.12 配置完成后，无法打开 Qt 源文件解决方案](https://blog.csdn.net/weixin_47156401/article/details/120626400)
