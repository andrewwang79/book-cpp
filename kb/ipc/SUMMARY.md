# 进程间通信
* [Linux进程通信方式：pipe无名管道、fifo有名管道、共享内存映射、socket、消息队列](https://zhuanlan.zhihu.com/p/57454565)

## 管道
1. 管道通常指无名管道,读写都需访问磁盘进行I/O操作,速度慢,容量有限  
1. 它是半双工的(即数据只能在一个方向上流动),具有固定的读端和写端
1. 它只能用于具有亲缘关系的进程之间的通信(也是父子进程或者兄弟进程之间)  
1. 它可以看成是一种特殊的文件,对于它的读写也可以使用普通的read、write等函数。但是它不是普通的文件,并不属于其他任何文件系统,只存在于内存中  

## FIFO
1. FIFO也称为命名管道,读写都需访问磁盘进行I/O操作,速度慢,容量有限  
1. 可以在无关的进程之间交换数据,与无名管道不同  
1. 有路径名与之相关联,它以一种特殊设备文件形式存在于文件系统中  

## Socket
1. Socket即套接字,用于描述地址和端口,是一个通信链的句柄。应用程序通过socket向网络发出请求或者回应  
1. Socket编程有三种,流式套接字(SOCK_STREAM),数据报套接字(SOCK_DGRAM),原始套接字(SOCK_RAW);前两种较常用。基于TCP的socket编程是采用的流式套接字  
1. 在TCP/IP网络应用中,通信的两个进程相互作用的主要模式是客户端/服务器模式,即客户端向服务器发出请求,服务器接收请求后,提供相应的服务  
1. 服务端:建立socket,声明自身的端口号和地址并绑定到socket,使用listen打开监听,然后不断用accept去查看是否有连接,如果有,捕获socket,并通过recv获取消息的内容,通信完成后调用closeSocket关闭这个对应accept到的socket,如果不再需要等待任何客户端连接,那么用closeSocket关闭掉自身的socket  
1. 客户端:建立socket,通过端口号和地址确定目标服务器,使用Connect连接到服务器,send发送消息,等待处理,通信完成后调用closeSocket关闭socket  

## 消息队列
1. 消息队列,是消息的链接表,存放在内核中,速度快  
1. 一个消息队列由一个标识符(即队列ID)来标识  
1. 消息队列是面向记录的,其消息具有特定的格式以及特定的优先级  
1. 消息队列独立于发送与接收进程。进程终止时,消息队列及其内容并不会被删除  
1. 消息队列可以实现消息的随机查询,消息不一定要以先进先出的次序读取,也可以按消息的类型读取  

## [共享内存](https://cpp.wangyaqi.cn/#/kb/ipc/sharememory)
1. 共享内存(Shared Memory),指两个或多个进程共享一个给定的存储区  
1. 共享内存是最快的一种 IPC,因为进程是直接对内存进行存取  
1. 因为多个进程可以同时操作,所以需要进行同步  
1. 信号量+共享内存通常结合在一起使用,信号量用来同步对共享内存的访问  

## [信号量](https://cpp.wangyaqi.cn/#/kb/ipc/semaphore)
1. 信号量(semaphore)是一个计数器,信号量用于实现进程间的互斥与同步,而不是用于存储进程间通信数据  
1. 信号量用于进程间同步,若要在进程间传递数据需要结合共享内存  
1. 信号量基于操作系统的PV操作,程序对信号量的操作都是原子操作  
1. 每次对信号量的PV操作不仅限于对信号量值加1或减1,而且可以加减任意正整数  
1. 支持信号量组  