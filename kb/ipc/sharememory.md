# 共享内存
> 共享内存的方式原理就是将一份物理内存映射到不同进程各自的虚拟地址空间上,这样每个进程都可以读取同一份数据,从而实现进程通信。因为是通过内存操作实现通信,因此是一种最高效的数据交换方法  

## C++基础库
### CreateFileMapping - 创建内存文件映射对象
```
HANDLE CreateFileMapping(  
  HANDLE hFile,                                  // 物理文件句柄  
  LPSECURITY_ATTRIBUTES lpFileMappingAttributes, // 安全设置  
  DWORD flProtect,                               // 文件数据访问方式  
  DWORD dwMaximumSizeHigh,                       // 高位文件大小  
  DWORD dwMaximumSizeLow,                        // 低位文件大小  
  LPCTSTR lpName                                 // 共享内存名称  
);
```

1. hFile:用于标识你想要映射到进程地址空间中的文件句柄,如果hFile为INVALID_HANDLE_VALUE,则调用进程还必须在dwMaximumSizeHigh和dwMaximumSizeLow参数中指定文件映射对象的大小。在这种情况下,CreateFileMapping创建一个指定大小的文件映射对象,该对象由系统分页文件而不是文件系统中的文件支持  
1. lpFileMappingAttributes:指向文件映射内核对象的SECURITY_ATTRIBUTES结构的指针,用于确定子进程是否可以继承返回的句柄,通常传递的值是NULL,表示不能继承该句柄,并且该文件映射对象将获得默认的安全描述符  
1. flProtect:对内存映射文件的安全设置(PAGE_READONLY 以只读方式打开映射;PAGE_READWRITE 以可读、可写方式打开映射;PAGE_WRITECOPY 只读或写时复制方式打开映射)  
1. dwMaximumSizeHigh:文件映射的最大长度的高32位  
1. dwMaximumSizeLow:文件映射的最大长度的低32位。如这个参数和dwMaximumSizeHigh都是零,就用磁盘文件的实际长度  
1. lpName:指定文件映射对象的名字,别的进程就可以用这个名字去调用 OpenFileMapping 来打开这个 FileMapping 对象  
1. 如果创建成功,返回创建的内存映射文件的句柄,如果已经存在,则也返回其句柄,但是调用 GetLastError()返回的错误码是:183(ERROR_ALREADY_EXISTS),如果创建失败,则返回NULL  
1. CreateFileMapping()函数创建过文件内核对象和文件映射内核对象,在进程终止之前有必要通过CloseHandle()将其释放,否则将会出现资源泄漏的问题  

* 示例

```
HANDLE hMapFile;  
// TCHAR szName[] = TEXT("Global\\MyFileMappingObject"); Global全局内存映射 新建需管理员权限  
TCHAR szName[] = TEXT("Local\\MyFileMappingObject");  

//创建共享文件句柄  
hMapFile = CreateFileMapping(INVALID_HANDLE_VALUE,  // 物理文件句柄  
                            NULL,                   // 默认安全级别  
                            PAGE_READWRITE,         // 可读可写  
                            0,                      // 高位文件大小  
                            1024,                   // 低位文件大小  
                            szName                  // 共享内存名称  
                            );  
```

### MapViewOfFile - 映射共享内存视图
```
LPVOID   MapViewOfFile(  
  HANDLE  hFileMappingObject,   //已创建共享内存的句柄  
  DWORD   dwDesiredAccess,      //文件数据的访问方式  
  DWORD   dwFileOffsetHigh,     //文件映射起始偏移的高32位  
  DWORD   dwFileOffsetLow,      //文件映射起始偏移的低32位  
  DWORD   dwNumberOfBytesToMap  //文件的映射长度  
);
```

1. hFileMappingObject:CreateFileMapping()返回的文件映像对象句柄  
1. dwDesiredAccess: 映射对象的文件数据的访问方式,而且同样要与CreateFileMapping()函数所设置的保护属性相匹配  
1. dwFileOffsetHigh: 表示文件映射起始偏移的高32位  
1. dwFileOffsetLow: 表示文件映射起始偏移的低32位  
1. dwNumberOfBytesToMap :文件中要映射的字节数。为0表示映射整个文件映射对象  

* 示例

```
LPCTSTR pBuf;  
//映射缓存区视图,获取指向共享内存的指针
pBuf = (LPTSTR)MapViewOfFile(hMapFile,             // 已经创建的共享文件句柄  
                            FILE_MAP_ALL_ACCESS,   // 访问模式:可读写  
                            0,                     // 文件偏移的高32位  
                            0,                     // 文件偏移的低32位  
                            1024                   // 映射视图大小  
                            );  
```

### OpenFileMapping - 打开共享内存视图
```
HANDLE OpenFileMapping(  
 DWORD dwDesiredAccess,   //文件数据的访问方式  
 BOOL bInheritHandle,     //函数返回的句柄是否可由子进程继承  
 LPCTSTR lpName           //共享内存名称  
);
```

1. dwDesiredAccess:同MapViewOfFile函数的dwDesiredAccess参数  
1. bInheritHandle :如这个函数返回的句柄能由当前进程启动的新进程继承,则这个参数为TRUE  
1. lpName :指定要打开的文件映射对象名称  
1. OpenFileMapping()函数打开文件内核对象和文件映射内核对象,在进程终止之前有必要通过CloseHandle()将其释放,否则将会出现资源泄漏的问题  

* 示例
```
HANDLE hMapFile;  
TCHAR szName[] = TEXT("Local\\MyFileMappingObject");  

hMapFile = OpenFileMapping(FILE_MAP_ALL_ACCESS,   // 访问模式:可读写  
                            FALSE,                //返回的句柄不能由当前进程启动的新进程继承  
                            szName);              // 共享内存名称  
```

## UnmapViewOfFile - 停止内存映射
```
BOOL UnmapViewOfFile{  
 LPCVOID lpBaseAddress  
};  
```

1. lpBaseAddress:指定要解除映射的一个文件映射的基准地址。这个地址是早先用MapViewOfFile函数获得的  
1. 在当前应用程序的内存地址空间解除对一个文件映射对象的映射  
1. 返回值:Bool,非零表示成功,零表示失败。可以通过GetLastError()获取错误代码  

* 示例
```
UnmapViewOfFile(pBuf);  
```

### CloseHandle() - 关闭句柄对象
```
BOOL CloseHandle(  
HANDLE hObject //句柄对象  
);
```

1. hObject :代表一个已打开的句柄对象  
1. 返回值:TRUE为执行成功;FALSE为执行失败,可以调用GetLastError()获知失败原因  

* 示例

```
CloseHandle(hMapFile);
```

## 场景
* 说明：P是生产者，C是消费者
* C返回的对象建议用P提供的共享内存【C创建的共享内存回传给P很不方便】

| 场景 | 步骤 | 结果 | 说明 |
| :-: | - | - | - |
| 1 | P启动写内存(字符串+byte/地址)，P同步拉起C，C读内存并覆盖写内存 | P正常打印内存 |  |
| 2 | P启动写内存，P同步拉起C，C读内存并写新内存 | P正常打印新内存 |  |
| 3 | P启动写内存，P异步拉起C，P循环打印内存里进度并等待C的结束通知，C覆盖写内存(进度每秒10%)，C完成后发出结束通知 | P看到C成功结束 |  |
| 4 | P启动写内存，P同步拉起C，C异常退出 | P收到失败和异常信息 |  |
| 5 | P启动写内存，P同步拉起C，P异常退出，C读内存并覆盖写内存 | C正常完成 | C在P异常退出前已经打开共享内存，就可以一直读写共享内存 |
| 6 | P和C启动，P写内存并通知C执行任务X，C在执行完成后通知P任务X执行完成 | P看到X执行成功 |  |

* 部分伪码

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

## 自定义封装类
* 定义：共享内存类**SM**，共享内存对象**SMO**，业务对象**BO**
* 需要转换的原因：业务对象不一定全部要传输，需要传输的数据有指针
* 生产时先创建共享内存后做转换的原因：如先做转换则无共享内存，相当于还要把转换后的结果再次拷贝到共享内存，增加耗时。

### 流程
1. 生产
    1. 基于SM的size创建共享内存。获取SMO(映射到共享内存)
    1. BO转换成SM结构，保存到SMO
1. 消费
    1. 基于SM的size开通共享内存，从共享内存读取SMO
    1. 把SMO转换到BO

### 共享内存的类含指针的传输方案
> 核心难点是指针对应的地址不在共享内存上，所以只能通过数据传输

1. SM里没有指针，每个指针单独做成1个共享内存：消费者基于指针对应数据的size开通共享内存，通过void*获取指针数据
1. SM是1个共享内存，用固定size的数据(如数组)替代指针，注意size要满足最大需求
