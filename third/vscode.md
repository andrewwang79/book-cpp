# vscode
## 插件
| 用途 | 名称 |
| - | - |
| 远程 | Remote - SSH |
| C++开发包 | C/C++ Extension Pack |
| 格式化 | Clang-Format |
| Java开发包 | Extension Pack for Java |
| Markdown查看 | Markdown Preview Enhanced |
| GIT | GitLens |

### GitLens
* [取消每行后面代码变更记录](https://blog.csdn.net/z284949127/article/details/111297448)

## 远程
* [Windows下设置SSH免密](https://www.jianshu.com/p/06f3b2d1ebdc)，首次用cmd登录(有confirm)
* [ssh连接用的是windows用户名的解决](https://blog.csdn.net/LittleSeedling/article/details/120606054)
* 1个IP多端口时，需设置不同的昵称“Host”。配置文件参考：

```
Host 192.168.1.27-CPP
  HostName 192.168.1.27
  Port 10101
  User root

Host 192.168.1.27-HOST
  HostName 192.168.1.27
  Port 22
  User user
```

## misc
* [在VS Code中开启gdb的pretty-printer功能](https://blog.csdn.net/yanxiangtianji/article/details/80579236)
