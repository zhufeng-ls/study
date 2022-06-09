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

当 socket 变得可写的时候，

