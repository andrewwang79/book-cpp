# ACL框架
## 概要
* 官网：https://gitee.com/acl-dev/acl
* 跨平台（LINUX，WIN32，Solaris，MacOS，FreeBSD）网络通信与服务器开发框架；
* 支持 HTTP/MQTT/Redis/Memcache/SMTP/PING/Beanstalk/handler socket 等网络通信协议；
* 支持线程池、协程、进程池、非阻塞、触发器等多种服务器编程模型；
* 持 XML/JSON/MIME/BASE64/UUCODE/QPCODE/RFC2047 等常见格式编解码；还有其它更多有价值的功能。

## 资料
* acl自带工程向导, 通过运行向导acl_wizard, 选择相应的类型, 即可生成相应的应用程序脚手架
* [使用 acl::master_proc 类编写多进程服务器程序](https://www.iteye.com/blog/zsxxsz-1541954)
* [使用 acl_cpp 库编写多线程程序](https://www.iteye.com/blog/zsxxsz-1965246)
* [acl 服务器模块的部署](https://www.iteye.com/blog/zsxxsz-1773413)
* [使用 acl 库编写负载均衡服务的配置指南](https://www.iteye.com/blog/zsxxsz-2341693)

## 目录结构
| 一级 | 二级 | 说明 |
| - | - | - |
| bin |  | acl管理工具 |
| conf | main.cf | acl_master控制进程配置文件 |
| conf | service | 服务配置文件 |
| libexec |  | 服务可执行文件 |
| sh |  | 启动/停止控制脚本 |
| var | log | 日志，守护进程和服务进程的log都在此 |

## log
| 文件名 | 说明 |
| - | - |
| acl_master | 守护进程 |
| A.acl | 服务A进程的ACL日志，在acl的cf配置文件配置的 |
| A.out | 服务A进程console out |
| A.err | 服务A进程console error |
| A.log | 服务A进程业务使用的日志 |
