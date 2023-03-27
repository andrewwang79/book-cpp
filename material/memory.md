# 内存
## Linux内存
* [内存分配–malloc、brk、mmap](https://blog.csdn.net/gfgdsg/article/details/42709943)
* [十问 Linux 虚拟内存管理 (glibc) (一) ](https://zhuanlan.zhihu.com/p/26137521)
* [程序内存映像、磁盘映像的理解](http://blog.chinaunix.net/uid-9012903-id-2011435.html)
* [Linux 堆内存管理深入分析（上）](https://segmentfault.com/a/1190000005118060)

| 术语 | 说明 |
| :-: | - |
| buffer cache(块设备)，page cache(文件系统)	 | https://blog.51cto.com/alanwu/1122077，https://blog.csdn.net/haiross/article/details/39478959 |
| swap(虚拟内存) | * VIRT 标识这个进程可以使用的内存总大小, 包括这个进程真实使用的内存, 映射过的文件, 和别的进程共享的内存等 <br> * RES 标识这个这个进程真实占用内存的大小 <br> * SHR 标识可以和别的进程共享的内存和库大小 |

## 共享内存
* 场景：P是生产者，C是消费者

| 场景 | 步骤 | 结果 | 说明 |
| :-: | - | - | - |
| 1 | P启动写内存(字符串+byte/地址)，P同步拉起C，C读内存并覆盖写内存 | P正常打印内存 |  |
| 2 | P启动写内存，P同步拉起C，C读内存并写新内存 | P正常打印新内存 |  |
| 3 | P启动写内存，P异步拉起C，P循环打印内存里进度并等待C的结束通知，C覆盖写内存(进度每秒10%)，C完成后发出结束通知 | P看到C成功结束 |  |
| 4 | P启动写内存，P同步拉起C，C异常退出 | P收到失败和异常信息 |  |
| 5 | P启动写内存，P同步拉起C，P异常退出，C读内存并覆盖写内存 | C正常完成 | C在P异常退出前已经打开共享内存，就可以一直读写共享内存 |
| 6 | P和C启动，P写内存并通知C执行任务X，C在执行完成后通知P任务X执行完成 | P看到X执行成功 |  |

* 伪码
```
3P：
while(1) {
  print(进度);
  if (C结束通知)
    break;
  sleep(100);
}
3C：
loop(10)
  写进度()
结束通知

6P：
通知C执行任务X
wait_signal(任务X执行完成);

```

# 进程间通信
* [Linux进程通信方式：pipe无名管道、fifo有名管道、共享内存映射、socket、消息队列](https://zhuanlan.zhihu.com/p/57454565)
* [semaphore和mutex](https://blog.csdn.net/zwan0518/article/details/8761000)：semaphore是多元信号量，mutex是锁(二元信号量)
* [Windows进程同步之互斥量内核对象](https://blog.csdn.net/iteye_18480/article/details/82514068)
* [windows进程通信-共享内存和信号量](https://blog.csdn.net/xihuanniNI/article/details/78958346)
