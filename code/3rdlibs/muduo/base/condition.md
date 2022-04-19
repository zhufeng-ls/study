# 条件变量

条件锁，可以用它来实现简单的 `生产者-消费者` 模式，且需要线程锁过来加锁。

可以用在 线程池、阻塞队列中

## 实现

```c
pthread_cond_init
pthread_cond_wait
pthread_cond_signal
pthread_cond_broadcast
pthread_cond_destroy
```

