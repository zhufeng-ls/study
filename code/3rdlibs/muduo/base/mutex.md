# 锁

使用linux 的标准 api 函数, 且锁附加了当前所在线程的属性，用来判断加锁的线程是否是当前线程。

自己封装实现了智能锁（构造和析构函数里加锁和解锁）。

自己内部也封装一层，供Condition使用。当 condition 加锁时，也会更改锁的 当前 tid。

## 实现
```c
pthread_mutex_init
pthread_mutex_destroy
pthread_mutex_lock
pthread_mutex_unlock
```

## 优点

1. 为了编码匿名智能锁的产生， 使用宏定义的方式，将其引导到了另一处。
   ```c
   #define MutexLockGuard(x) error "Missing guard object name"
   ```