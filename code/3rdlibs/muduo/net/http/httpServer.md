# http server

## 入参

* EventLoop* loop: 事件循环队列
* InetAddress& listenAddr: 监听的IP地址
* string& name： 服务器的名称
* TcpServer::Option option： 服务器的模式，即复用端口和不复用端口

## setHttpCallback

设置 http 回调

## setThreadNum

设置 http 服务器中线程池的数量

## start

开启 http 服务器


## 注意

httpServer 的底层是采用 tcp 实现的， httpServer 拥有一个 tcpServer 的对象，作为它的主要调用对象， 所以回调函数都需要传入 tcp 类中。 