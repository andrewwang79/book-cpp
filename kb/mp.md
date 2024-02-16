# 多线程
* [线程锁和信号量](https://cpp.wangyaqi.cn/#/kb/ipc/semaphore)

## 线程的生命周期管理
* 显式地管理线程的生命周期，确保每个线程要么被join，要么被detach。
* 为线程的结束编写适当的清理代码，确保所有资源都被回收。
* 让主线程等待所有工作线程完成任务后再退出。

## 类
### std::thread

|    函数   |           作用                |
|-----------|------------------------------|
|void join()| 等待线程结束并清理资源(会阻塞)|
|bool joinable()|返回线程是否可以执行join函数|
|void detach()|将线程与调用其的线程分离,彼此独立执行(此函数必须在线程创建时立即调用,且调用此函数会使其不能被join)|
|std::thread::id get_id()|获取线程id     |

```c++
#include <iostream>
#include <thread>
using namespace std;
void doit() { cout << "World!" << endl; }
int main() {
 thread a([]{cout << "Hello, " << flush;});
 thread b(doit);
 a.join();
 b.join();
 return 0;
}
```

### std::async

```c++
#include <iostream>
#include <future> // std::async std::future

template<class ... Args> decltype(auto) sum(Args&&... args) {
 return (0 + ... + args); // "0 +"避免空参数包错误
}

int main() {
 std::future<int> val = std::async(std::launch::async, sum<int, int, int>, 1, 10, 100);
 std::future::get() // 阻塞等待线程结束并获得返回值
 cout << val.get() << endl;
 return 0;
}
```

### 线程池
1. **线程池管理器(ThreadPool):** 用于创建并管理线程池,包括 创建线程池,销毁线程池,添加新任务。
1. **工作线程(PoolWorker):** 线程池中线程,在没有任务时处于等待状态,可以循环的执行任务。
1. **任务接口(Task):** 每个任务必须实现的接口,以供工作线程调度任务的执行,它主要规定了任务的入口,任务执行完后的收尾工作,任务的执行状态等。
1. **任务队列(taskQueue):** 用于存放没有处理的任务。提供一种缓冲机制。
