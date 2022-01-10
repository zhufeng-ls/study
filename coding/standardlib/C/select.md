### select

参考网址:
https://blog.csdn.net/qq_39871498/article/details/90733415

适用于监听套接字较少的情况。

函数原型:
```
int select(int maxfd,fd_set *rdset,fd_set *wrset, \  
             fd_set *exset,struct timeval *timeout); 
```

参数说明:
1. 参数maxfd是需要监视的最大的文件描述符值+1
2. rdset,wrset,exset分别对应于需要检测的可读文件描述符的集合，可写文件描述符的集合及异常文件描述符的集合。
3. struct timeval结构用于描述一段时间长度，如果在这个时间内，需要监视的描述符没有事件发生则函数返回，返回值为0。
