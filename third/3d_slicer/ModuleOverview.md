# Module概览
> 本文仅作学习研究分享使用- 原文地址 https://slicer.readthedocs.io/en/latest/developer_guide/extensions.html

Slicer支持多种类型的模块:
* 命令行交互式 Command Line Interface (CLI)
* 可加载 Loadable
* 脚本 Scripted

对特定类型的模块的选择通常是基于特定模块的输入/参数类型。

## 命令行交互式模块

CLI是可独立执行，其输入/输出参数的复杂性有限（参数类型简单，没有用户互动......）。推荐用于实现计算算法。

特点：

* CLI = 命令行界面
* Slicer可以从GUI中运行任何命令行应用程序（通过提供接口描述.xml文件）。
* 可以用任何语言实现（C++，Python，...）。
* 在XML文件中指定输入和输出，自动生成GUI。
* 通过命令行和文件传递参数。
* 在一个单独的处理线程中运行，可以报告进度并被终止。
  
不支持（反模式）：
* 传回中间的结果。
* 在执行过程中更新视图。
* 在运行中接受输入，以控制模块。
* 在运行中请求输入。

#### Get Started
* 对于CLI，构建Slicer并使用扩展向导创建一个初始骨架，添加一个`cli`类型的模块。
* 对于脚本CLI，使用扩展向导创建一个初始骨架，添加一个`scriptedcli`类型的模块。

更多：

* [开发和贡献3D Slicer扩展](https://slicer.readthedocs.io/en/latest/developer_guide/extensions.html#extensions)
* [开发者教程](https://www.slicer.org/wiki/Documentation/Nightly/Training#Tutorials_for_software_developers)
* [Slicer执行模型](https://www.slicer.org/wiki/Documentation/Nightly/Developers/SlicerExecutionModel)，描述将命令行程序纳入Slicer模块的机制。
* 学习现有的Slicer [CLI 模块](https://github.com/Slicer/Slicer/tree/main/Modules/CLI)

## 可加载模块

可加载模块是针对Slicer构建的C++插件。他们为自己的特定行为定义了自定义的GUI，因为他们对应用程序有完全的控制权。

推荐用于实现复杂的、性能关键的、交互式的组件，应用基础设施（例如，低级别的可复用的GUI控件）。

特点：

* 用C++编写，并作为共享库分发。
* 可访问全部的[Slicer API](https://slicer.readthedocs.io/en/latest/developer_guide/api.html#c)
* 可控制Slicer用户界面（基于Qt）和Slicer内部（[MRML](MRML/MRMLOverview.md)、逻辑、显示管理器...）。
* 建立Slicer并使用扩展向导创建一个初始骨架，添加一个`loadable`类型的模块。

更多：
* [开发和贡献3D Slicer扩展](https://slicer.readthedocs.io/en/latest/developer_guide/extensions.html#extensions)
* [开发者教程](https://www.slicer.org/wiki/Documentation/Nightly/Training#Tutorials_for_software_developers)
* 学习现有的Slicer [loadable 模块](https://github.com/Slicer/Slicer/tree/main/Modules/Loadable)


## 脚本模块

脚本化模块是使用Slicer API的Python脚本。他们为自己的特定行为定义自定义图形用户界面，因为他们对应用程序有完全的控制权。

推荐用于快速原型设计和定制工作流程开发。

特点：

* 用Python编写。
* 可访问全部的[Slicer API](https://slicer.readthedocs.io/en/latest/developer_guide/api.html#c)
* 完全访问[MRML](MRML/MRMLOverview.md)、[VTK](https://vtk.org/)、[Qt](https://doc.qt.io/)、[ITK](https://itkpythonpackage.readthedocs.io/en/master/)和[SimpleITK](https://simpleitk.org/about.html)的API，因为它们都有Python封装。
* 学习现有的Slicer [脚本模块](https://github.com/Slicer/Slicer/tree/main/Modules/Scripted)

## 模块工厂

将模块加载到Slicer中需要经过多个步骤:

* 模块工厂必须被注册到工厂管理器中。
* 要加载的模块所在的目录必须传递给工厂管理器。
* 可选择指定要忽略的模块名称。
* 扫描目录，测试哪个文件是模块并注册（还没有实例化）。
* 实例化所有的注册模块。
* 将每个模块与场景和应用程序连接起来。

更多细节可在[在线文档](https://apidocs.slicer.org/main/classqSlicerAbstractModuleFactoryManager.html)中找到

## 关联MRML节点和模块

模块可以与MRML节点相关联，例如，这可以确定什么模块可以用来编辑某个MRML节点。一个模块可以通过覆盖`qSlicerAbstractCoreModule::associatedNodeTypes()`方法来指定它所支持的节点类型列表，或者一个模块可以调用`qSlicerCoreApplication::addModuleAssociatedNodeTypes()`来将任何节点类型与任何模块关联。

多个模块可以与同一个MRML节点类型相关联。编辑特定节点实例的最佳模块是在运行时确定的。应用框架为每个相关的候选模块调用`qSlicerAbstractModuleWidget::nodeEditable()`，并将激活对处理该节点有最高可能性的模块。

为了选择一个MRML节点作为模块中的 "活动 "或 "编辑 "节点，模块中widget的`qSlicerAbstractModuleWidget::setEditedNode()` 方法被调用。

## 远程模块
### 为什么要有远程模块
* 简洁 Slicer Core
* 允许个人或组织在自己的私人模块上工作，并可选择将这些模块提供给Slicer用户，而无需使用扩展管理器。

### 添加远程模块需要满足条件
* 已知该模块可在Linux、MacOSX和Windows上编译。
* 该模块已测试
* 该模块文档已在wiki上
* 该模块名称唯一
* 在未来的任何时候，Slicer主仓库中的模块都不应该依赖于远程模块。
* 远程模块`必须`定义一个特定的唯一版本（即git hash）。对于调试和科学可重复性来说，每个Slicer的修订版都有一套独特的代码，这一点很重要。

### 添加远程模块的步骤
1. 与Slicer 核心开发者联系讨论
2. 在`SuperBuild.make`中增加条目，使用`Slicer_Remote_Add()`宏。例如
    ```
    Slicer_Remote_Add(Foo
    GIT_REPOSITORY ${git_protocol}://github.com/awesome/foo
    GIT_TAG abcdef
    OPTION_NAME Slicer_BUILD_Foo
    LABELS REMOTE_MODULE
    )
    list_conditional_append(Slicer_BUILD_Foo Slicer_REMOTE_DEPENDENCIES Foo)
    ```
3. 对应的commit信息应该如：
   ```
    ENH: Add Foo remote module

    The Foo module provide the user with ...
   ```

`注意`

作为调用`Slicer_Remote_Add`的副作用，(1)选项`Slicer_BUILD_Foo`将被自动添加为高级选项，(2)CMake变量`Slicer_BUILD_Foo`和`Foo_SOURCE_DIR`将被传递给Slicer内部构建。

此外，通过指定`REMOTE_MODULE`标签，在`Slicer/Modules/Remote/CMakeLists.txt`中，相应的源代码目录将通过调用`add_directory`自动被添加。

`Slicer_Remote_Add`在`Slicer/Modules/Remote`中创建一个源内模块目标。远程模块的SuperBuild目标只运行源码更新步骤，没有单独的构建步骤。

### 更新远程模块的步骤
1. 更新SuperBuild.cmake中的条目
2. 用类似于以下的信息进行commit：
```
ENH: Update Foo remote module

List of changes:

$ git shortlog abc123..efg456
John Doe (2):
    Add support for ZZZ spacing
    Refactor space handler to support multi-dimension
```