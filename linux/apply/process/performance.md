## time

可以用来测试程序执行的时间，精度是毫秒。

real: 表示实际执行的时间，包括IO堵塞 和 其他进程使用的时间片

user: 表示用户态执行的时间，不包括 阻塞和其它进程使用的时间片

sys: 内核所消耗的时间

## 注意

1. real 有的时候会比user小？
    
    因为多线程和多进程时，程序可以并行执行。