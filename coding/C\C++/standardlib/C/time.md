### 获取时间

timeval 和 timespec 用来获取更高精度的时间。
```
#include<sys/time.h>
int  clock_gettime(clockid_t,struct timespec *) // 用来获取特定时钟的时间。
```

时钟类型如下：
CLOCK_REALTIME 统当前时间，从1970年1.1日算起。
CLOCK_MONOTONIC 系统的启动时间，不能被设置。
CLOCK_PROCESS_CPUTIME_ID 本进程运行时间。
CLOCK_THREAD_CPUTIME_ID 本线程运行时间。