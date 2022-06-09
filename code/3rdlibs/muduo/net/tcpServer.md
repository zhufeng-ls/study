# TcpServer

## 参数

* loop_: 主事件循环
* ipPort_: 监听的ip端口
* name_: tcpserver 名称，也会传入到 eventLoopThreadPool 中
* acceptor_ : 监听的服务器，由传入的 loop_ 和 ipPort_ 做参数进行创建
* threadPool_: io线程池
* connectionCallback_: 每个socket 建立连接时触发的回调，传入到 TcpConnection 中
* messageCallback_： 消息到达触发的回调，传入到 TcpConnection 中
* nextConnId_: 下一个io线程的id

## 操作

### 1. 在构造函数中初始化上述参数，

### 2. 给 acceptor 绑定新连接进入的回调

### 3. 设置io线程池数量
```c++
void TcpServer::setThreadNum(int numThreads)
{
  assert(0 <= numThreads);
  threadPool_->setThreadNum(numThreads);
}
```

### 4. 开启 tcpServer

确保start 是第一次调用
```c++
if (started_.getAndSet(1) == 0)
{

}
```

开启线程池，并在当前 loop_ 中开启 acceptor 的监听
```c++
threadPool_->start(threadInitCallback_);

assert(!acceptor_->listening());
loop_->runInLoop(
    std::bind(&Acceptor::listen, get_pointer(acceptor_)));
```

### newConnection

当 Acceptor 监听到新的连接，并建立了和客户端的 socket 时，触发此回调。

首先获取下个io线程的事件循环 ioLoop ，即将每个新连接，依次放到每个io线程中。
```c++
EventLoop* ioLoop = threadPool_->getNextLoop();
```

创建一个新的TcpConnection 对象，且是用智能指针管理的，并设置它的连接成功的回调，消息到达的回调，消息发送完毕的回调，消息关闭的回调。
```c++
TcpConnectionPtr conn(
    new TcpConnection(ioLoop, connName, sockfd, localAddr, peerAddr));
connections_[connName] = conn;
conn->setConnectionCallback(connectionCallback_);
conn->setMessageCallback(messageCallback_);
conn->setWriteCompleteCallback(writeCompleteCallback_);
conn->setCloseCallback(
    std::bind(&TcpServer::removeConnection, this, _1));  // FIXME: unsafe
```

在新的 io 线程中运行连接建立的回调。
```c++
ioLoop->runInLoop(std::bind(&TcpConnection::connectEstablished, conn));
```

这里面主要是启动channel 的监听，同时运行上述传入的 connectionCallback_ 回调。


## removeConnection

tcpServer 主动断开某个 TcpConnection 连接。

从本地容器中擦出TcpConnection 连接， 再到对应的 io 线程中主动调用 connectDestroy来断开连接。

```c++
void TcpServer::removeConnectionInLoop(const TcpConnectionPtr& conn) {
  loop_->assertInLoopThread();
  LOG_INFO << "TcpServer::removeConnectionInLoop [" << name_
           << "] - connection " << conn->name();
  size_t n = connections_.erase(conn->name());
  (void)n;
  assert(n == 1);
  EventLoop* ioLoop = conn->getLoop();
  ioLoop->queueInLoop(std::bind(&TcpConnection::connectDestroyed, conn));
}
```

connectDestroyed 的实现

首先取消 socket 的监听，然后从 loop_ 中移除对应的channel。
```c++
size_t n = connections_.erase(conn->name());
(void)n;
assert(n == 1);
EventLoop* ioLoop = conn->getLoop();
ioLoop->queueInLoop(std::bind(&TcpConnection::connectDestroyed, conn));
```



removeConnectionInLoop 并不会断开 TcpConnection 的连接，它只是停止对 TcpConnection 的监听，断开连接需调用 TcpConnection::shutdown();