# ITK
* [最佳实践代码](https://github.com/andrewwang79/cpp.practice/tree/master/itk)
* [医学图像分割与配准（ITK实现分册1）](./book/医学图像分割与配准（ITK实现分册1）.pdf), [医学图像分割与配准（ITK实现分册2）](./book/医学图像分割与配准（ITK实现分册2）.pdf)

## 资料
* [医学图像处理工具包ITK](https://blog.csdn.net/clheang/article/details/44220843)
* [SimpleITK学习（一）基本概念](https://www.cnblogs.com/wzyuan/p/10493136.html)
* [教程](https://so.csdn.net/so/search?q=itk&t=blog&u=weixin_44671418)
* [ITK图像读写详解.pdf](https://max.book118.com/html/2018/0322/158204784.shtm)

### 官方
* https://itk.org/ItkSoftwareGuide.pdf
* [build](https://itk.org/Wiki/ITK/Getting_Started/Build/Linux)
* [hello world](https://itk.org/Wiki/ITK/HelloInsight)

## 编码
1. ITK的Pointer作为函数输出参数**须是引用**：如fn(itk::Image<float, 3>::Pointer& image)