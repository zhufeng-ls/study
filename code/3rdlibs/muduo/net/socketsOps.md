# SocketsOps 

定义了许多对 socket 套接字操作的函数。

## createNonblockingOrDie

创建一个非阻塞的 tcp 流套接字，且 拥有 close_exec 属性。
```c
int sockfd = ::socket(family, SOCK_STREAM | SOCK_NONBLOCK | SOCK_CLOEXEC, IPPROTO_TCP);
```

## bindOrDie


## listenOrDie

SOMAXCONN: 库提供的最大连接的套接字数量

## accept

底层采用的 accept4。

```c
int connfd = ::accept4(sockfd, sockaddr_cast(addr),
                         &addrlen, SOCK_NONBLOCK | SOCK_CLOEXEC)
```

accept4 和 accept 的区别就是多了第四个设置的参数。多的那部分功能可以用 fcntl 代替， 但是 fcntl 在多线程中可能会存在竞争，而通过accept4 就可以直接在打开的文件描述符上设置。

## 关闭写操作

```c++
if (::shutdown(sockfd, SHUT_WR) < 0)
{
LOG_SYSERR << "sockets::shutdownWrite";
}
```

## setsockopt

基本可以设置 socket 所有的属性, fcntl 包含 setsockopt。

### 设置心跳

```c++
int optval = on ? 1 : 0;
::setsockopt(sockfd_, SOL_SOCKET, SO_KEEPALIVE,
            &optval, static_cast<socklen_t>(sizeof optval));
```

## getsocket 

获取 socket 的详细信息


## setsockopt、fcntl、ioctl 的不同

* ioctl设置与IO设备高度相关的属性
* fcntl设置文件描述符的属性
* setsockopt设置网络套接字的属性
 
总体而言设备范围逐渐缩小，所设置的属性更具有网络编程针对性。

ioctl用于硬件设备I/O通道控制，控制命令和参数都与设备高度相关，通常也与系统高度相关。

ioctl是底层的系统调用，所以跨平台性不好（可能某些平台的IO属性在其他平台上并不具有）；而fcntl是封装好的函数，各个OS都是支持的。



## 关闭套接字

传统的方法有 close()，closesocket()，这个是关闭双方的连接，调用后，双方完全无法使用与收发有关联的函数。

优雅的方法可以使用 shutdown

```c
int shutdown(int sock, int howto); 
```

* howto 选择断开的流
  * SHUT_RD：断开输入流。套接字无法接收数据（即使输入缓冲区收到数据也被抹去），无法调用输入相关函数。
  * SHUT_WR：断开输出流。套接字无法发送数据，但如果输出缓冲区中还有未传输的数据，则将传递到目标主机。
  * SHUT_RDWR：同时断开 I/O 流。相当于分两次调用 shutdown()，其中一次以 SHUT_RD 为参数，另一次以 SHUT_WR 为参数。


### close()/closesocket()和shutdown()的区别

确切地说，close() / closesocket() 用来关闭套接字，**将套接字描述符（或句柄）从内存清除，之后再也不能使用该套接字**，与C语言中的 fclose() 类似。应用程序关闭套接字后，与该套接字相关的连接和缓存也失去了意义，TCP协议会自动触发关闭连接的操作。

shutdown() 用来**关闭连接，而不是套接字**，不管调用多少次 shutdown()，套接字依然存在，直到调用 close() / closesocket() 将套接字从内存清除。

## 设置端口可复用

```c++
int ret = ::setsockopt(sockfd_, SOL_SOCKET, SO_REUSEPORT,
                        &optval, static_cast<socklen_t>(sizeof optval));
```