## socket类型

### SOCK_SEQPACKET

报文段，若每次只读取部分字节，则未读取完毕的会扔掉

### SOCK_STREAM

流输出，每次可任意读取，流读取会在本地创建一个缓冲区，

---
---
## 注意：

### 关于socket设置成阻塞和非阻塞的区别

阻塞和非阻塞的区别在于没有数据到达的时候，阻塞会一直等待，不阻塞的则会理解返回。

非阻塞的用法是select + accept ，阻塞的用法只是accept。

当只有一个socket连接时，没有必要用非阻塞，

当有多个socket连接时，需要用到非阻塞，不然当执行读操作时，若没有数据，则会一直等待，其他的socket则无法工作，若为非阻塞，使用select 监控，没有数据会立即返回，哪个socket 有数据就会先执行哪一个，即循环执行活跃的socket。

### 设置非阻塞的方法

```
//获取文件的flags
  fcntl(serverFd, F_GETFL, &flags);
//增加文件的某个flags，比如文件是阻塞的，想设置成非阻塞:
  fcntl(serverFd, F_SETFL, O_NONBLOCK | flags);
```

### shutdown()

```
#include<sys/socket.h>

int shutdown(int sockfd,int how);

how的方式有三种分别是

SHUT_RD（0）：关闭sockfd上的读功能，此选项将不允许sockfd进行读操作。不允许接受。

SHUT_WR（1）：关闭sockfd的写功能，此选项将不允许sockfd进行写操作。不允许发送。



SHUT_RDWR（2）：关闭sockfd的读写功能。不允许发送和接受(和 close() 一样)。

成功则返回0，错误返回-1，错误码errno：EBADF表示sockfd不是一个有效描述符；ENOTCONN表示sockfd未连接；ENOTSOCK表示sockfd是一个文件描述符而不是socket描述符。
```

### socketpair

```
#include <sys/types.h>
#include <sys/socket.h>
int socketpair(int d, int type, int protocol, int sv[2])；

参数1（domain）：表示协议族，在Linux下只能为AF_LOCAL或者AF_UNIX。（自从Linux 2.6.27后也支持SOCK_NONBLOCK和SOCK_CLOEXEC）
* SOCK_CLOEXEC: 在 fork 后，会在子进程中关闭已在父进程中打开的此套接字。

参数2（type）：表示协议，可以是SOCK_STREAM或者SOCK_DGRAM。SOCK_STREAM是基于TCP的，而SOCK_DGRAM是基于UDP的
参数3（protocol）：表示类型，只能为0
参数4（sv[2]）：套节字柄对，该两个句柄作用相同，均能进行读写双向操作
返回结果： 0为创建成功，-1为创建失败，并且errno来表明特定的错误号，具体错误号如下所述：

创建一对无名的、相互连接的套接字。
1. 这对套接字可以用于全双工通信，每一个套接字既可以读也可以写。例如，可以往sv[0]中写，从sv[1]中读；或者从sv[1]中写，从sv[0]中读； 
2. 如果往一个套接字(如sv[0])中写入后，再从该套接字读时会阻塞，只能在另一个套接字中(sv[1])上读成功； 
3. 读、写操作可以位于同一个进程，也可以分别位于不同的进程，如父子进程。如果是父子进程时，一般会功能分离，一个进程用来读，一个用来写。因为文件描述副sv[0]和sv[1]是进程共享的，所以读的进程要关闭写描述符, 反之，写的进程关闭读描述符。 
```

## readv 和 writev

用来一次性读或写多个非连续的缓冲区

```c
#include <sys/uio.h>
ssize_t readv(int filedes, const struct iovec *iov, int iovcnt);
ssize_t writev(int filedes, const struct iovec *iov, int iovcnt);
// 两个函数的返回值：若成功则返回已读、写的字节数，若出错则返回-1
```

### 参数

* filedes: 文件描述符
* iov: 指向 iovec 数组的头部指针
* opvcnt: iov 数组中元素的个数。