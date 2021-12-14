

## select 的用法

```
int select(int maxfdp,fd_set *readfds,fd_set *writefds,fd_set *errorfds,struct timeval *timeout);
```

fd_set 为存放文件描述符的集合，即当socket 有 I/O事件到来时，存放到相应的readfds和writefds中。可以通过FD_ZERO(fd_set *)来清空。

将一个给定的文件描述符加入集合之中FD_SET(int ,fd_set
)；
将一个给定的文件描述符从集合中删除FD_CLR(int
,fd_set)；

