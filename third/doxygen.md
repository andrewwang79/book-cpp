# doxygen
* 采用[Doxygen](https://blog.csdn.net/Candy1232009/article/details/80786179)编写C++文档，[Doxygen官方所有指令](https://www.doxygen.nl/manual/commands.html)
* [vscode 自动生成Doxygen格式注释](https://www.its404.com/article/ABC_ORANGE/119538802)
* [C++项目代码实例](https://github.com/andrewwang79/cpp.practice/tree/master/doxygen)

![](../s/third/doxygen.png)

## 编写
### 文件
```
/**
 * @file 文件名
 * @brief 简介
 * @details 细节
 * @author 作者
 * @email 邮箱
 * @date 年.月.日
 * @copyright Copyright (c) 2022@SKY
 * @license 版权
 */
```

### 类
```
/**
 * @brief 类简介，比如功能说明
 * @details 细节
 * @warning 警告
 * @attention 注意事项
 * @note 备注说明。可用于类修改记录
 * | 时间 | 作者 | 说明 |
 * | :-: | - | - |
 * | 20210.05.12 | wangyaqi | 增加函数xxx |
 */
```

### 函数
```
/**
 * @brief 函数简介，比如功能说明
 * @param
 * | 名称 | 字段 | 类型 | 必须 | 说明 |
 * | :-: | - | - | - | - |
 * | 序列号 | sn | string | Y | 唯一 |
 * | 引用字段A[out] | refA | string& | Y | 说明 |
 * @return 返回值说明
 *   @retval 0 成功
 *   @retval -1 失败
 * @example 使用示例
 * @exception 超出范围的异常
 * @warning 警告
 * @attention 注意事项
 * @note 备注说明。可用于函数修改记录
 * | 时间 | 作者 | 说明 |
 * | :-: | - | - |
 * | 20210.05.12 | wangyaqi | 增加输入参数xxx |
 */
```

### API接口
* 在函数前加，参见https://github.com/andrewwang79/cpp.practice/blob/master/doxygen/DogService.h

```
/**
 * @brief  接口简介，比如功能说明
 * @date   2021.05.12
 * @author wangyaqi
 * @param
 * | 名称 | 字段 | 类型 | 必须 | 说明 |
 * | :-: | - | - | - | - |
 * | 类型 | type | string | Y | 类型枚举 |
 * | 序列号 | sn | string | Y | 唯一 |
 * @return
 * | 名称 | 字段 | 类型 | 必须 | 说明 |
 * | :-: | - | - | - | - |
 * | 结果 | result | string | Y | 结果情况 |
 * | 区域 | detection/polygon | array<double> | Y | 2D点 |
 * 返回码
 * | 返回码 | 名称 | 说明 |
 * | :-: | - | - |
 * | 1001 | 图像读取错误 |  |
 * | 1002 | 图像不合法 |  |
 * @warning   警告
 * @attention 注意事项
 * @note 备注说明。可用于函数修改记录
 * | 时间 | 作者 | 说明 |
 * | :-: | - | - |
 * | 20210.05.12 | wangyaqi | 增加输入参数xxx |
 */
```

### 文档的主页内容
* 在任意文件添加，参见https://github.com/andrewwang79/cpp.practice/blob/master/doxygen/main.cpp

```
/*! \mainpage 主页
# 简介
* 本内容显示在首页
* 简单的markdown格式，完全自定义
# Quick Start
一些使用说明。
*/
```

### 文档的导航树页面
* 在项目里添加md文件新增导航树的项，参见https://github.com/andrewwang79/cpp.practice/blob/master/doxygen/article.md

### 生成UML图
* 安装graphviz，确保以下参数。参考自https://blog.csdn.net/Cross_Entropy/article/details/117265884
```
EXTRACT_ALL            = YES
HAVE_DOT               = YES
UML_LOOK               = YES
```

## 构建
* 确保输入待分析目录是干净的，只有代码，没有编译过程文件等！
* 通过配置模板文件[Doxyfile.tpl](https://github.com/andrewwang79/cpp.practice/blob/master/doxygen/Doxyfile.tpl)构建，[脚本](https://github.com/andrewwang79/cpp.practice/blob/master/doxygen/gen.sh)使用变量替换并生成

```
#!/bin/bash
set -e

if [ ! $# == 5 ]; then
  echo "usage : sh gen.sh 产品名 版本 输入待分析目录 输出文档目录 doxyfile模板文件"
  echo "example : sh gen.sh taihang 1.0.0.0 code doc Doxyfile.tpl"
  exit 0
fi

PRODUCT_NAME=$1
FULL_VERSION_NUM=$2
INPUT_DIRECTORY=$3
OUTPUT_DIRECTORY=$4
DOXYFILE_TPL=$5

apt-get install -y doxygen graphviz
DOXYFILE=$DOXYFILE_TPL.file
cp -f $DOXYFILE_TPL $DOXYFILE
sed -i "s#%productName%#${PRODUCT_NAME}#g" ${DOXYFILE}
sed -i "s#%fullVersionNum%#${FULL_VERSION_NUM}#g" ${DOXYFILE}
sed -i "s#%inputDirectory%#${INPUT_DIRECTORY}#g" ${DOXYFILE}
sed -i "s#%outputDirectory%#${OUTPUT_DIRECTORY}#g" ${DOXYFILE}

doxygen ${DOXYFILE}

rm $DOXYFILE
```
