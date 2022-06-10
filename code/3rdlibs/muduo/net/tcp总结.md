# Tcp 总结 

## 为什么 TcpConnection 要接收回调？

因为这个对象同时承担 client 和 server 的新连接，所以处理连接的方式自然也会有不同，所以要接收不同的回调，同时 channel 应该和这个对象绑定在一起，再添加到事件循环中，也是因为它有不同回调的原因。

## TcpServer 和 TcpClient 的 newConnection 

TcpServer 的 newConnection 传入到 Acceptor ,当有新连接到来的时候则调用。

TcpClient 的 newConenction 传入到 Connector， 当成功建立了和服务器的连接时调用。


## TcpServer

### 作用 
* 创建多个 io 线程，每个线程依次处理新到的客户端连接。
* 设置开启的 io 线程数量
* 新连接到来时服务端的回调
  * 通过添加一个新的 TcpConnection 对象到事件循环中
  * 且这个回调是传入到 Acceptor 中的。
* 移除客户端的连接
  * 通过主动调用 TcpConnection 中的 connectDestroyed 函数，来销毁连接，客户端主动断开连接和服务端断开客户端的连接，操作都是一样的，所有将这个共有的操作封装到 TcpConnection 中。
  * connectDestroyed 内部的主要操作是从 poll 中解除监听，并从 loop 中移除 channel，并不会关闭这个 Connection 的连接。
  * 若想关闭这个 TcpConnection 的连接，则还需要手动调用 TcpConnection::shutdown()


## Acceptor

主要用来监听新的连接，即将 accept 的操作封装成了一个类，所以每个 TcpServer 都有一个 Acceptor。

### 作用

* 监听新的连接
  * 内部使用了封装的 ::listen。
  * 使用 channel::enableReading 将 监听的 socket 添加到 poll 中。 
* handleRead
  * 新连接到来时触发的回调
  * 内部会执行 newConnectionCallback_, 由 TcpServer 传入，因为 Acceptor 只监听新的连接，而建立的新连接如何处理就由 TcpServer 来操心，所以需要这个回调。


## TcpConnection

用来关联 socketfd 和 其对应的 channel, 并为 channel 设置各种事件对应的回调函数。

### 作用

* 获取 tcp 连接的信息
  * getTcpInfo
  * getTcpInfoString
* 发送数据
  * 若在当前 io 线程中，则直接发送，
  * 若不在，则通过 loop 的 runInLoop 进行发送，
  * 这样做的好处就是发送都在一个线程中，不需要担心线程安全的问题。
  * 发送数据时，若没有发送完，则继续监听 socketfd 的可写事件，当可写的时候，则继续从 buffer 中取数据发送。
* 断开连接
  * 断开连接的操作也在 loop 中进行，这样就不会存在 tcp 的状态不一致的问题了。
  * **这边关闭连接是关闭这边的写通道，然后等到对方主动关闭连接，触发这边的关闭回调时，这个时候两边都关闭了，剩下的就是将 socketfd 和 关联的 channel 从 loop 和 poll 移除**
  * 断开的回调是从 TcpServer 或 TcpClient 中传入的。
* 强制关闭连接
  * 正常的是接收到客户端的关闭请求在触发回调，这个是直接手动触发回调
  * 要和 shutdown 配套使用，就可以不用等待对方，而直接关闭本地的连接了。
* 延时关闭连接
  * 通过定时器的 runAfter 函数。
* startRead、stopRead
  * 开启和关闭读事件的监听
* connectEstablished， connectDestroyed
  * 即将 TcpServer 和 TcpClient 的收尾操作封装起来， 他们也是在触发的回调函数内部被调用的。
  * 函数内部大致都是对 channel 的操作。
* handleRead, handleWrite, handleClose, handleError
  * 传入 channel，不同的事件触发时，调用不同的回调。


## Connector 

将客户端请求连接的操作封装成一个类。

## 作用

* 开始连接服务器
  * 内部调用了 ::connect, 
  * 同时生成了一个 channel， 并监听它的可写事件。
* removeAndResetChannel
  * 重置用来监听新连接是否可写的 channel。
* handleWrite
  * 当和服务端的连接可写的时候，则当前监听的 channel 的使命完成了，删除 chanel, 并触发 newConnectionCallback_
  * newConnectionCallback_ 是 TcpClient 传入的回调，这个回调的参数是 socketfd, 内部创建了一个和 socketfd 绑定的 新的 channel。
  * 只有当客户端和服务器建立的连接变得可写的时候，我们才认为连接建立成功。
* retry 
  * 重连服务器，每次尝试重连的间隔可以自己设置。


## TcpClient

tcp 客户端，内部包含了 Connector 连接类。

### 作用

* 连接
  * 连接的操作封装在 Connector中
* 断开连接
  * 调用 TcpConnection 中的 shutdown 来主动断开
* 停止连接
  * 停止与服务器的重连，原理是停止对 socketfd 的写事件的监听，也封装在了 Connector 中。
