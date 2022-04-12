# 开发规范
## 文件格式
1. 编码是UTF8
1. LineEnd是LF

## log
```
// 带参数的格式。标题 : key1[value1], keyX[valueX]
LOG_DEBUG_FMT("result : code[%s], length[%d]", code.c_str(), 100);
```

## 编码规范
1. 采用google规范
  * vscode：
  * VisualStudio：工具 -> 选项 -> 文本编辑器 -> C/C++ -> 格式设置 -> 默认格式设置样式 -> Google
1. 不需要80字符换行
1. 项目头文件用""记载，其他头文件用<>

## 提交
1. 代码提交前必须格式化
1. 代码格式化的单独提交，不要和业务混在一起
1. 在README.md记录修改内容，比如新增目录的用途
1. 配套的UT代码

## UT
* [单元测试](/dev/ut)

## 文档编写
* [Doxygen](/third/doxygen)
