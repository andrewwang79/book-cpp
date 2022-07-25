# 内存泄漏

## 内存知识
* [内存分配–malloc、brk、mmap](https://blog.csdn.net/gfgdsg/article/details/42709943)
* [十问 Linux 虚拟内存管理 (glibc) (一) ](https://zhuanlan.zhihu.com/p/26137521)
* [程序内存映像、磁盘映像的理解](http://blog.chinaunix.net/uid-9012903-id-2011435.html)
* [Linux 堆内存管理深入分析（上）](https://segmentfault.com/a/1190000005118060)
* [linux 进程通信方式 pipe无名管道 fifo有名管道 共享内存映射 socket 消息队列](https://zhuanlan.zhihu.com/p/57454565)
* [一次"内存泄漏"引发的血案](https://www.jianshu.com/p/38a4bcf564d5)

| 术语 | 说明 |
| :-: | - |
| buffer cache(块设备)，page cache(文件系统)	 | https://blog.51cto.com/alanwu/1122077，https://blog.csdn.net/haiross/article/details/39478959 |
| swap(虚拟内存) |  |

### swap(虚拟内存)
* VIRT 标识这个进程可以使用的内存总大小, 包括这个进程真实使用的内存, 映射过的文件, 和别的进程共享的内存等. 
* RES 标识这个这个进程真实占用内存的大小. 
* SHR 标识可以和别的进程共享的内存和库大小
