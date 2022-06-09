# EventLoopThread

这个类的意义是在一个新的线程创建一个事件循环，并返回事件循环的对象指针。

实现原理，是通过在线程的回调函数中创建一个局部对象 EventLoop, 在通过 loop.loop() 堵塞线程，确保局部对象不会被释放掉，再把局部对象的指针传出来。
```c++
void EventLoopThread::threadFunc()
{
    EventLoop loop;

    if (callback_)
    {
        callback_(&loop);
    }

    {
        MutexLockGuard lock(mutex_);
        loop_ = &loop;
        cond_.notify();
    }

    loop.loop();
    // assert(exiting_);
    MutexLockGuard lock(mutex_);
    loop_ = NULL;
}
```

### 传入参数

* const ThreadInitCallback& cb: 即创建事件循环前用来初始化的函数
* name: 新的线程名称


### startLoop

线程的启动，但有个精巧的地方，通过条件变量确保事件循环loop创建成功。

```c++
EventLoop* loop = NULL;
{
    MutexLockGuard lock(mutex_);
    while (loop_ == NULL) { cond_.wait(); }
    loop = loop_;
}
```
