# Connector

Connector 是为 TcpClient 所调用的，一个客户端需要一个套接字来与服务端通信，muduo 使用Connector 来封装这个与服务端通信的套接字（如同Acceptor 封装了客户端连接套接字）。

## 参数

* loop_ : 事件循环
* serverAddr: 连接的服务器地址
* connect_： 是否尝试连接
* state_: 当前的连接状态
* retryDelayMs_： 每次尝试重连的间隔


## start 

开启连接,内部主要调用了 startInLoop

startInLoop:

判断当前状态是否为断开状态，并判断是否开启了尝试连接，若开启了，则连接
```c++
loop_->assertInLoopThread();
assert(state_ == kDisconnected);
if (connect_)
{
    connect();
}
else
{
    LOG_DEBUG << "do not connect";
}
```

connect 内部调用了 ::connect 函数，并为新的 socket 绑定的 channel 设置写回调和错误回调函数，并添加到 poller 中，监听它的可写事件。

> 只有当 socketfd 变得可写的时候，我们才认为连接建立成功。

```c++
setState(kConnecting);
assert(!channel_);
channel_.reset(new Channel(loop_, sockfd));
channel_->setWriteCallback(
    std::bind(&Connector::handleWrite, this)); // FIXME: unsafe
channel_->setErrorCallback(
    std::bind(&Connector::handleError, this)); // FIXME: unsafe

// channel_->tie(shared_from_this()); is not working,
// as channel_ is not managed by shared_ptr
channel_->enableWriting();
```


## handleWrite

当 socket 变得可写的时候，先判断当前 socket 的连接状态，然后将Connector 绑定的 channel 断掉，然后运行 newConnectionCallback_ 回调函数。
```c++
if (state_ == kConnecting)
{
    int sockfd = removeAndResetChannel();
    int err    = sockets::getSocketError(sockfd);
    if (err)
    {
        LOG_WARN << "Connector::handleWrite - SO_ERROR = " << err << " "
                    << strerror_tl(err);
        retry(sockfd);
    }
    else if (sockets::isSelfConnect(sockfd))
    {
        LOG_WARN << "Connector::handleWrite - Self connect";
        retry(sockfd);
    }
    else
    {
        setState(kConnected);
        if (connect_)
        {
            newConnectionCallback_(sockfd);
        }
        else
        {
            sockets::close(sockfd);
        }
    }
}
```

newConnectionCallback_ 回调是 TcpClient 传入的，传入的参数为 socketfd,

其内部主要是创建一个 TcpConnection 对象，并将 socketfd 和 TcpConnection 对象内部的 channel 绑定，然后设置 channel 的回调。
```c++
TcpConnectionPtr conn(new TcpConnection(loop_,
                                        connName,
                                        sockfd,
                                        localAddr,
                                        peerAddr));

conn->setConnectionCallback(connectionCallback_);
conn->setMessageCallback(messageCallback_);
conn->setWriteCompleteCallback(writeCompleteCallback_);
conn->setCloseCallback(
    std::bind(&TcpClient::removeConnection, this, _1)); // FIXME: unsafe
{
    MutexLockGuard lock(mutex_);
    connection_ = conn;
}
```

然后再运行 connectEstablished 回调函数， 这里与 TcpServer 不同的是， TcpServer 运行 connectEstablished 是在对应的 io 线程中运行的，而 TcpClient 则是当前线程运行的。
```c++
//TcpServer
ioLoop->runInLoop(std::bind(&TcpConnection::connectEstablished, conn));

//TcpClient
conn->connectEstablished();
```

