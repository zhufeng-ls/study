# TcpConnection

TcpConnection 是服务器接收到客户端的请求，并建立了连接，生成了 socket, 并为新的 socket 进行了一层封装。

## sendInLoop

发送数据。

判断是否监听了socket 的写事件，已经outputBuffer_ 是否为空，若为真，则直接写入。若写完了所有的数据，则调用 writeCompleteCallback_。

```c++
if (!channel_->isWriting() && outputBuffer_.readableBytes() == 0)
{
nwrote = sockets::write(channel_->fd(), data, len);
if (nwrote >= 0)
{
    remaining = len - nwrote;
    if (remaining == 0 && writeCompleteCallback_)
    {
    loop_->queueInLoop(std::bind(writeCompleteCallback_, shared_from_this()));
    }
}
```

若一个字节都没有写入，则对错误进行下一步判断
```c++
nwrote = 0;
if (errno != EWOULDBLOCK)
{
    LOG_SYSERR << "TcpConnection::sendInLoop";
    if (errno == EPIPE || errno == ECONNRESET) // FIXME: any others?
    {
        faultError = true;
    }
}
```

若不是 faultError , 且待写的字节超过了高水位标记，则调用高水位标记函数。
```c++
if (!faultError && remaining > 0)
{
    size_t oldLen = outputBuffer_.readableBytes();
    if (oldLen + remaining >= highWaterMark_
        && oldLen < highWaterMark_
        && highWaterMarkCallback_)
    {
        loop_->queueInLoop(std::bind(highWaterMarkCallback_, shared_from_this(), oldLen + remaining));
    }
}
```

将剩余的字节添加到 outputBuffer_, 并开始监听 socket 的可写事件。


## handleWrite

socket 变得可写时，触发写回调。
```c++
void TcpConnection::handleWrite()
{
  loop_->assertInLoopThread();
  if (channel_->isWriting())
  {
    ssize_t n = sockets::write(channel_->fd(),
                               outputBuffer_.peek(),
                               outputBuffer_.readableBytes());
  }
}
```

从 outputBuffer_ 回收已写的字节。若写完了，则关闭写事件的监听，并开始调用 writeCompleteCallback_

```c++
if (n > 0)
{
    outputBuffer_.retrieve(n);
    if (outputBuffer_.readableBytes() == 0)
    {
    channel_->disableWriting();
    if (writeCompleteCallback_)
    {
        loop_->queueInLoop(std::bind(writeCompleteCallback_, shared_from_this()));
    }
    }
}
```

写完了后，还要对 socket 的状态进行一次判断，判断 socket 是否关闭，若正在关闭的过程中，则改变 socket 的输出状态。
```c++
if (state_ == kDisconnecting)
{
    shutdownInLoop();
}
```

## handleRead

读数据进入 inputBuffer 缓冲区，并调用设置好的消息到达的回调函数。
```c++
ssize_t n = inputBuffer_.readFd(channel_->fd(), &savedErrno);
if (n > 0)
{
    messageCallback_(shared_from_this(), &inputBuffer_, receiveTime);
}
```

若读取的字节数为0，则关闭socket
```c
if (n == 0)
{
    handleClose();
}
```

## handleClose

先将socket 状态置为 kDisconnected, 再将对应的 channel 监听的事件取消
```c++
setState(kDisconnected);
channel_->disableAll();
```

然后调用设置的回调，先调用connectionCallback_, 这里面会对当前的连接信息进行一个打印， 然后调用 closeCallback_, 这里面主要就是关闭的回调，由 tcpServer 传入，函数主体为: TcpServer::removeConnectionInLoop
```c++
TcpConnectionPtr guardThis(shared_from_this());
connectionCallback_(guardThis);
// must be the last line
closeCallback_(guardThis);
```

## 客户端

connectDestroyed 和 connectEstablished 是连接主动断开或建立时调用的函数，并不是回调。