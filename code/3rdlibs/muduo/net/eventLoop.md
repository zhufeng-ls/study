# EventLoop

## 1. 什么都不做的EventLoop

一个EventLoop就是一个事件循环，下面将通过一个“什么都不做的EventLoop”来大致描述下muduo中EventLoop的功能。

“什么都不做的EventLoop”有如下几个特点：

* one loop per thread意思是说每个线程最多只能有一个EventLoop对象。
* EventLoop对象构造的时候，会检查当前线程是否已经创建了其他EventLoop对象，如果已创建，终止程序（LOG_FATAL）
* EventLoop构造函数会记住本对象所属线程（threadId_）。
* 创建了EventLoop对象的线程称为IO线程，其功能是运行事件循环（EventLoop::loop）

## 2. eventloop 的创建和启动

检查线程中是否已经存在一个事件循环了
```c++
EventLoop::EventLoop()
  : looping_(false),
    threadId_(CurrentThread::tid())
{
  LOG_TRACE << "EventLoop created " << this << " in thread " << threadId_;
  // 如果当前线程已经创建了EventLoop对象，终止(LOG_FATAL)
  if (t_loopInThisThread)
  {
    LOG_FATAL << "Another EventLoop " << t_loopInThisThread
              << " exists in this thread " << threadId_;
  }
  else
  {
    t_loopInThisThread = this;
  }
}
```

若存在则终止
```c++
void assertInLoopThread()
{
  if (!isInLoopThread())
  {
    abortNotInLoopThread();
  }
}

bool isInLoopThread() const { return threadId_ == CurrentThread::tid(); }

void EventLoop::abortNotInLoopThread()
{
  LOG_FATAL << "EventLoop::abortNotInLoopThread - EventLoop " << this
            << " was created in threadId_ = " << threadId_
            << ", current thread id = " <<  CurrentThread::tid();
} 
```

## 事件循环

事件循环的底层是依赖于 poll 和 epoll、


### eventfd

eventfd 是一个用来通知事件的文件描述符，类似于管道，

1. 创建一个 eventfd 对象，或者说打开一个 eventfd 的文件，类似普通文件的 open 操作。
    ```c
    ::eventfd(0, EFD_NONBLOCK | EFD_CLOEXEC);
    ```

对于eventfd，只有一个系统调用接口
```c
int eventfd(unsigned int initval, int flags);
```
创建一个eventfd对象，或者说打开一个eventfd的文件，类似普通文件的open操作。

该对象是一个内核维护的无符号的64位整型计数器。初始化为initval的值。

flags可以以下三个标志位的OR结果：

* EFD_CLOEXEC：FD_CLOEXEC，简单说就是fork子进程时不继承，对于多线程的程序设上这个值不会有错的。
* EFD_NONBLOCK：文件会被设置成O_NONBLOCK，一般要设置。
* EFD_SEMAPHORE：（2.6.30以后支持）支持semophore语义的read，简单说就值递减1。



