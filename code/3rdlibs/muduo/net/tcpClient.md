# TcpClient

## 传入参数

* loop: loop 事件循环
* serverAddr: 服务器地址
* nameArg: 客户端的名称

## 构造函数初始化

* loop_ : TcpClient 的 socketfd 的事件监听运行在这个 loop 中。
* connector_: 用来处理和服务器的连接
* name_ ： 客户端名称
* connectionCallback_: 连接或断开时，用来进行日志打印的回调，传入到 TcpConnection 中。
* messageCallback_： 消息到达时的回调，传入到 TcpConnection 中。
* retry_： 是否重连
* connect_： 是否连接。
* nextConnId_： 下一个 io 线程的 id。

## connect 

连接服务器

通过调用 connector_ 的 start 函数，即连接的操作封装在了 Connector 中。
```c++
connect_ = true;
connector_->start();
```

## disconnect

断开与服务器的连接

通过调用 connection_ 的 shutdown() 函数, 断开的操作是通用的，所以封装在了 TcpConnection 中，且要加锁保证 connection_ 的安全。
```c++
MutexLockGuard lock(mutex_);
if (connection_)
{
    connection_->shutdown();
}
```

## stop

停止连接服务器，通过调用 Connector 的 stop 函数

```c++
connector_->stop();
```

## newConenction

传入到 Connector 中， 内部创建了一个 TcpConnector ,并绑定回调
```c++
TcpConnectionPtr conn(
    new TcpConnection(loop_, connName, sockfd, localAddr, peerAddr));

conn->setConnectionCallback(connectionCallback_);
conn->setMessageCallback(messageCallback_);
conn->setWriteCompleteCallback(writeCompleteCallback_);
conn->setCloseCallback(
    std::bind(&TcpClient::removeConnection, this, _1)); // FIXME: unsafe
{
    MutexLockGuard lock(mutex_);
    connection_ = conn;
}
conn->connectEstablished();
```


## removeConnection

是一个传入 TcpConnection 的回调，调用的其实也是 TcpConnenction 内部的 connectDestroyed 函数，并判断 retry_ ,确定是否需要进一步重连。
```c++
loop_->queueInLoop(std::bind(&TcpConnection::connectDestroyed, conn));
if (retry_ && connect_)
{
    LOG_INFO << "TcpClient::connect[" << name_ << "] - Reconnecting to "
                << connector_->serverAddress().toIpPort();
    connector_->restart();
}
```



