# 文件操作

## unlink
```c
#include <unistd.h>

int unlink(const char * pathname);
```

删除 pathname 指向的路径。

如何为符号链接，则直接删除，若为硬链接，且有其他进程打开了文件，则等到文件描述符关闭时才进行删除。

返回值:
* 0: 成功
* -1： 失败，错误原因在 errno。


## setbuffer

设置文件缓冲区的大小,缓冲区满了就刷新
```c
g_file = fopen("/dev/null", "w");
setbuffer(g_file, buffer, sizeof buffer);
```