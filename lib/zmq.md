# ZMQ
> 非常喜欢ZMQ的理念

* [介绍](https://dongshao.blog.csdn.net/article/details/105984928)
* [消息模式](https://blog.csdn.net/qq_41453285/article/details/106865539)
* 基于zmq的C++库cppzmq

## 开发
1. 标准的Socket需求用PAIR的消息模式，两个节点双向异步收发二进制数据，建议接收在独立线程执行。
