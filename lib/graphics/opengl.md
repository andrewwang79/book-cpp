# OpenGL
* 渲染2D、3D矢量图形的跨语言、跨平台的应用程序编程接口（API）。用来绘制从简单的图形比特到复杂的三维景象，常用于CAD、虚拟现实、科学可视化程序和电子游戏开发。
* 是图形库，没有UI。面向过程的C函数库
* [OpenGL入门教程](https://blog.csdn.net/xiangzhihong8/article/details/84776943)
* https://zhuanlan.zhihu.com/p/56693625
* https://zhuanlan.zhihu.com/p/374920616

## 配套库
* https://blog.csdn.net/qq_42741249/article/details/125737731

| 项 | 说明 | 资料 |
| - | - | - |
| GLEW(OpenGL Extension Wrangler Library) | 类似glad。对底层OpenGL接口的封装，可以让调用代码跨平台 |  |
| GLFW(GL framework) | FreeGLUT升级版。窗口库，主要是视窗界面支持(窗口、键盘等)。 |  |
| GLUT/FreeGLUT | gl utility toolkit。freeglut3-dev |  |
| OpenGL ES(OpenGL for Embedded Systems) | 三维图形API的子集，针对手机、PDA和游戏主机等嵌入式设备而设计 |  |
| WebGL(Web Graphics Library) | 3D绘图协议，通过增加OpenGL ES的一个JavaScript绑定，WebGL可以为HTML5 Canvas提供硬件3D加速渲染 |  |

## 坐标空间
1. 局部空间(Local Space，或者称为物体空间(Object Space))
1. 世界空间(World Space)
1. 观察空间(View Space，或者称为视觉空间(Eye Space))
1. 裁剪空间(Clip Space)
1. 屏幕空间(Screen Space)
