# Acceptor 

用来监听客户端连接的，即内部封装了 accept。

## 实现

1. 通过 listen 监听 socket
```c++
void Acceptor::listen()
{
  loop_->assertInLoopThread();
  listening_ = true;
  acceptSocket_.listen();
  acceptChannel_.enableReading();
}
```
2. 当 socket 有读事件产生时，调用 accept 接收连接，若没有注册回调函数，则直接关闭新的连接。
```c++
void Acceptor::handleRead()
{
  loop_->assertInLoopThread();
  InetAddress peerAddr;
  //FIXME loop until no more
  int connfd = acceptSocket_.accept(&peerAddr);
}
```

3. 当请求的连接过多的时候, accept 就会返回 EMFILE 错误，表示没有可用的描述符。

```c++
if (errno == EMFILE)
{
    ::close(idleFd_);
    idleFd_ = ::accept(acceptSocket_.fd(), NULL, NULL);
    ::close(idleFd_);
    idleFd_ = ::open("/dev/null", O_RDONLY | O_CLOEXEC);
}
```

先关闭临时的描述符，此时空出来一个描述符，然后用空出来的描述符接收新的请求，接收到请求后将其关闭，再将临时的描述符给暂存起来，供后续使用。

