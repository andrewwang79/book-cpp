# 信号量

## CreateMutex - 创建互斥量对象
```
CreateMutex用来创建一个有名或无名的互斥量对象,其函数原型为:  
HANDLE CreateMutex(  
        LPSECURITY_ATTRIBUTES lpMutexAttributes,  //允许继承的默认描述符  
        BOOL bInitialOwner,                       //初始化互斥对象的所有者  
        LPCTSTR lpName                            //设置互斥对象的名字  
);
```

1. lpMutexAttributes:指定一个SECURITY_ATTRIBUTES结构,或传递NULL,表示使用不允许继承的默认描述符
1. bInitialOwner:初始化互斥对象的所有者。TRUE为占有,操作系统记录线程ID,将递归计数器设置为1,互斥量处于未触发/无信号/未通知状态。FALSE为不占有,线程ID为NULL,操作系统将递归计数器设置为0,互斥量处于触发/有信号/已通知状态,不为任何线程所占用,一个互斥体同时只能由一个线程拥有  
1. lpName:指向互斥对象名的指针,设置互斥对象的名字。创建一个未命名的互斥体对象。如已经存在这个名字,则打开已命名互斥体。有了名字就可以跨进程得到同一把锁

* 示例

```
HANDLE hMutex;  
TCHAR mutex_name[] = TEXT("SM_Mutex");  
hMutex = CreateMutex(NULL, FALSE, mutex_name);  //创建一个互斥量对象,且当前进程不占有此互斥体
```

## OpenMutex - 打开互斥量对象
```
OpenMutex为现有的一个已命名互斥体对象创建一个新句柄  
HANDLE OpenMutex(  
DWORD dwDesiredAccess,   // 访问权限  
BOOL bInheritHandle,     // 是否可由子进程继承  
LPCTSTR lpName           // 互斥对象的名字  
);
```

1. dwDesiredAccess:MUTEX_ALL_ACCESS 请求对互斥体的完全访问;MUTEX_MODIFY_STATE 允许使用 ReleaseMutex 函数;SYNCHRONIZE 允许互斥体对象同步使用  
1. bInheritHandle:函数返回的句柄是否可由子进程继承  
1. lpName:要打开对象的名字  
1. 返回值:如执行成功,返回对象的句柄;零表示失败。若想获得更多错误信息,请调用GetLastError函数  
1. 一旦不再需要,注意一定要用 CloseHandle 关闭互斥体句柄。如对象的所有句柄都已关闭,那么对象也会删除  

* 示例

```
HANDLE hFileMapping = NULL;  
TCHAR mutex_name[] = TEXT("SM_Mutex");  
hMutex = OpenMutex(MUTEX_ALL_ACCESS, FALSE, mutex_name);  
```

## WaitForSingleObject - 等待获取互斥量对象
```
DWORD WaitForSingleObject(  
HANDLE hHandle,         //等待的对象句柄  
DWORD dwMilliseconds    //等待时长  
);
```

1. hHandle:对象句柄。可以指定一系列的对象,如Event、Job、Memory resource notification、Mutex、Process、Semaphore、Thread、Waitable timer等  
1. dwMilliseconds:等待时长,单位为milliseconds(毫秒).如果指定一个非零值,函数处于等待状态直到hHandle标记的对象被触发,或者时间到了。如果dwMilliseconds为0,对象没有被触发信号,函数不会进入一个等待状态,它总是立即返回。如果dwMilliseconds为INFINITE,对象被触发信号后,函数才会返回  

* 示例

```
if (WaitForSingleObject(hMutex, 5000) != WAIT_OBJECT_0) // 5秒内hMutex是否处于有信号状态,则有则继续下面的代码  
{  
return 0; //如无则停止等待,函数返回  
}  
```

## ReleaseMutex - 释放互斥量对象
* ReleaseMutex用来释放互斥量对象,其函数原型为ReleaseMutex(HANDLE hMutex);  

1. ReleaseMutex()让调用进程释放对该互斥体的拥有权,此时另一个在等待该mutex的线程,将得到信号停止等待(wait函数)  
1. 访问完共享资源以后应该释放对Mutex对象的拥有权,让其他要访问共享资源的线程拥有这个Mutex对象  
