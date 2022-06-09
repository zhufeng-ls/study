# 定时器

muduo的定时器由三个类实现，TimerId、Timer、TimerQueue，用户只能看到第一个类，其它两个都是内部实现细节

## 可选择的定时器方案

sleep
alarm
usleep
nanosleep
clock_nanosleep
getitimer / setitimer
timer_create / timer_settime / timer_gettime / timer_delete
timerfd_create / timerfd_gettime / timerfd_settime
muduo选择了timerfd_*系列作为定时器方案，下面是入选的几个原因：

sleep / alarm / usleep 在实现时有可能用了信号 SIGALRM，在多线程程序中处理信号是个相当麻烦的事情，应当尽量避免
nanosleep 和 clock_nanosleep 是线程安全的，但是在非阻塞网络编程中，绝对不能用让线程挂起的方式来等待一段时间，程序会失去响应。正确的做法是注册一个时间回调函数。
getitimer 和 timer_create 也是用信号来 deliver 超时，在多线程程序中也会有麻烦。 timer_create 可以指定信号的接收方是进程还是线程，算是一个进步，不过在信号处理函数(signal handler)能做的事情实在很受限。
**timerfd_create 把时间变成了一个文件描述符，该“文件”在定时器超时的那一刻变得可读**，这样就能很方便地融入到 select/poll 框架中，用统一的方式来处理 IO 事件和超时事件，这也正是 Reactor 模式的长处。

## timer

```c
#include <sys/timerfd.h>

int timerfd_create(int clockid, int flags);

int timerfd_settime(int fd, int flags, const struct itimerspec *new_value, struct itimerspec *old_value);

int timerfd_gettime(int fd, struct itimerspec *curr_value)
```

### 1. timerfd_create
```c
int timerfd_create(int clockid, int flags);
```

它是用来创建一个定时器描述符timerfd

* 第一个参数：clockid指定时间类型，有两个值：

    * CLOCK_REALTIME :Systemwide realtime clock. 系统范围内的实时时钟

    * CLOCK_MONOTONIC:以固定的速率运行，从不进行调整和复位 ,它不受任何系统time-of-day时钟修改的影响

* 第二个参数：flags可以是0或者O_CLOEXEC/O_NONBLOCK。

* 返回值：timerfd（文件描述符）

### 2、timerfd_settime
```c
int timerfd_settime(int fd, int flags, const struct itimerspec *new_value, struct itimerspec *old_value);
```
该函数作用是用来启动或关闭有fd指定的定时器。在讲解该函数前，先理解两个相关结构体：
```c
struct timespec {
    time_t tv_sec;      /* Seconds */
    long   tv_nsec;     /* Nanoseconds */
};

struct itimerspec {
    struct timespec it_interval;  /* Interval for periodic timer */
    struct timespec it_value;     /* Initial expiration */
};
```
第二个结构体itimerspec就是timerfd要设置的超时结构体，它的成员it_value表示定时器第一次超时时间，it_interval表示之后的超时时间即每隔多长时间超时

下面正式分析该函数的参数和返回值：

参数：

* fd：timerfd，有timerfd_create函数返回
* flags：1代表设置的是绝对时间；为0代表相对时间
* fnew_value：指定新的超时时间，设定new_value.it_value非零则启动定时器，否则关闭定时器，如果new_value.it_interval为0，则定时器只定时一次，即初始那次，否则之后每隔设定时间超时一次

* old_value：不为null，则返回定时器这次设置之前的超时时间

### 3、timerfd_gettime
```c
int timerfd_gettime(int fd, struct itimerspec *curr_value)
```
此函数用于获得定时器距离下次超时还剩下的时间。如果调用时定时器已经到期，并且该定时器处于循环模式（设置超时时间时struct itimerspec::it_interval不为0），那么调用此函数之后定时器重新开始计时。

### 参数

* const TimerCallback callback_; //定时执行函数
* Timestamp expiration_;  // 超时时间
* const double interval_; // 执行间隔
* const bool repeat_;  // 是否重复定时执行任务
* const int64_t sequence_; // 序列号，每个Timer的序列号都不一样


sequence 是通过一个静态变量的原子增加来表示独一无二的， 且该原子操作是再 Timer 函数的构造函数初始化列表中完成初始化
```c
static AtomicInt64 s_numCreated_;
```

### 重置设置超时时间

使用 restart(Timestamp now) 函数
```c++
void restart(Timestamp now);

void Timer::restart(Timestamp now)
{
  if (repeat_)
  {
    expiration_ = addTime(now, interval_);
  }
  else
  {
    expiration_ = Timestamp::invalid();
  }
}
```

## TimerId

TimerId 顾名思义就是用来保存 Timer 类的信息，包含 timer 对象 和 id(序列号)，同时将 TimerQueue 声明为友元类。

TimerId 也是创建定时器时返回的对象，创建者可通过这个返回的对象对创建的对象进行操作，所以TimerId 更像是对内部的一层封装。


## TimerQueue

通过 timerfd_settime 设置定时器的超时时间，到时候后 timerfd 会变得可读，从而触发它的可读事件。

### 构造函数初始化

通过 timerfd_create 生成一个 timerfd,  并通过 timerfd 创建一个 timerfdChannel, 并在函数内部设置读事件的回调函数。


### 析构函数

关闭 timerfd_, timerfdChannel, 释放 timer 资源。

```c++
timerfdChannel_.disableAll();
timerfdChannel_.remove();
::close(timerfd_);
// do not remove channel, since we're in EventLoop::dtor();
for (const Entry& timer : timers_) { delete timer.second; }
```

### addTimer

通过runInLoop 运行 addTimerInLoop, 确保添加定时器的操作在事件循环中，这样就不存在死锁的问题了。

```c++
TimerId TimerQueue::addTimer(TimerCallback cb, Timestamp when, double interval)
{
    Timer* timer = new Timer(std::move(cb), when, interval);
    loop_->runInLoop(std::bind(&TimerQueue::addTimerInLoop, this, timer));
    return TimerId(timer, timer->sequence());
}
```

### cancel

传入 timerId , 同时在 事件循环中执行 cancelInLoop 操作。

#### cancelInLoop

先检查 activeTimers_ 中是否能找到 timerId 中的 timer 对象，若能找到，则分别从 timers_ 和 activeTimers_ 剔除, 并释放掉 timer 对象。
```c++
ActiveTimer timer(timerId.timer_, timerId.sequence_);
ActiveTimerSet::iterator it = activeTimers_.find(timer);
if (it != activeTimers_.end())
{
    size_t n = timers_.erase(Entry(it->first->expiration(), it->first));
    assert(n == 1);
    (void)n;
    delete it->first;
    activeTimers_.erase(it);
}
```

再检查当前是否有定时器事件触发，若是有，则将定时器对象添加到 cancelingTimer_ 的容器中, 此时取消定时器，也只能等待触发的定时器事件执行完毕后才可行。
```c++
if (callingExpiredTimers_)
{
    cancelingTimers_.insert(timer);
}
```

### reset

当定时器执行完毕后，重置定时器，因为有些定时器是反复执行的，这个时候检查需要重置的定时器是否在 等待删除的定时器cancelingTimer_列表中，免得又重复添加了想要删除的定时器。
```c++
ActiveTimer timer(it.second, it.second->sequence());
if (it.second->repeat() &&
    cancelingTimers_.find(timer) == cancelingTimers_.end())
{
    it.second->restart(now);
    insert(it.second);
}
else
{
    delete it.second;
}
```

当已经触发了的定时器列表里，没有我们想要删除的定时器，这个时候我们就不可能会把它重新添加到链表中，所以就不需要做任何处理了。

### getExpired

从中获取已经超时了的定时器。

通过从存放定时器的有序集合 timers_ 中，通过 set::low_bound 找到已经触发的定时器列表，诸葛对他们进行处理，并将他们从 timers_ 和 erase 中删除, 后续可能会在 reset() 重新添加。
```c++
Entry sentry(now, reinterpret_cast<Timer*>(UINTPTR_MAX));
TimerList::iterator end = timers_.lower_bound(sentry);
assert(end == timers_.end() || now < end->first);
std::copy(timers_.begin(), end, back_inserter(expired));
timers_.erase(timers_.begin(), end);

for (const Entry& it : expired)
{
    ActiveTimer timer(it.second, it.second->sequence());
    size_t n = activeTimers_.erase(timer);
    assert(n == 1);
    (void)n;
}
```

前面没有在剔除的地方对循环的定时器做过滤的原因，是为了代码的可读性，将添加和剔除彻底分开来，不然在剔除时，做太多的条件判断，会严重影响可读性。