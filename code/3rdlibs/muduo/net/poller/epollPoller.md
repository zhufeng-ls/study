# epoll 监控

判断 epoll 宏和 poll 宏是否相同, poll(2) 和 epoll(4) 是一致的
```c++
static_assert(EPOLLIN == POLLIN,        "epoll uses same flag values as poll");
static_assert(EPOLLPRI == POLLPRI,      "epoll uses same flag values as poll");
static_assert(EPOLLOUT == POLLOUT,      "epoll uses same flag values as poll");
static_assert(EPOLLRDHUP == POLLRDHUP,  "epoll uses same flag values as poll");
static_assert(EPOLLERR == POLLERR,      "epoll uses same flag values as poll");
static_assert(EPOLLHUP == POLLHUP,      "epoll uses same flag values as poll");
```

## 实现

* epoll_create1(EPOLL_CLOEXEC)： 和 epoll_create类似，即创建一个epoll的事例。当创建成功后，会占用一个fd，所以记得在使用完之后调用close()，否则fd可能会被耗尽。
  * EPOLL_CLOEXEC: 用来设置 exec 后 fd 的状态的，若设置了这个属性，则 exec 后的进程， fd 会被关闭
  * EPOLL_NONBLOCK： epfd 会被设置为非阻塞

* close(epollfd_): 关闭 epoll 事例
* epoll_wait: 等待活跃事件的发生,若监听到的事件均触发，则给 events_(存放所有的注册事件) 进行2倍扩容。
  ```c++
  int numEvents = ::epoll_wait(epollfd_,
                               &*events_.begin(),
                               static_cast<int>(events_.size()),
                               timeoutMs);
  ```
* epoll_ctl: 对需要监听的 fd 进行操作，比如添加进入监听列表，或者从监听列表中删除，或者修改监听的 fd 对应的 data


使用 epoll_event 的data参数 存储 channel 指针，这样可以快速定位到fd 对应的 channel。


## updateChannel

检查 channel 的状态，若为 kNew(还没有被添加) 或 kDelete(已经从epoll里被删除)时，则重新添加 channel 到 epoll
```c++
if (index == kNew || index == kDeleted)
{
    // a new one, add with EPOLL_CTL_ADD
    int fd = channel->fd();
    if (index == kNew)
    {
        assert(channels_.find(fd) == channels_.end());
        channels_[fd] = channel;
    }
    else // index == kDeleted
    {
        assert(channels_.find(fd) != channels_.end());
        assert(channels_[fd] == channel);
    }

    channel->set_index(kAdded);
    update(EPOLL_CTL_ADD, channel);
}
```

若为 kAdded, 表明channel 已经注册到 epoll 中， 检查监听的事件，若没有监听任何事件，则从 epoll 中删除，否则从 epoll 中修改 channel 的属性。
```c++
int fd = channel->fd();
(void)fd;
assert(channels_.find(fd) != channels_.end());
assert(channels_[fd] == channel);
assert(index == kAdded);
if (channel->isNoneEvent())
{
    update(EPOLL_CTL_DEL, channel);
    channel->set_index(kDeleted);
}
else
{
    update(EPOLL_CTL_MOD, channel);
}
```

## removeChannel

确保 channel 没有监听任何事件, 所以删除 channel 之前，可以将监听的事件取消
```c++
assert(channel->isNoneEvent());
```

确保 channel 的状态
```c++
assert(index == kAdded || index == kDeleted);
```

分别从 channels_ 和 epoll 中删除事件
```c++
size_t n = channels_.erase(fd);
(void)n;
assert(n == 1);

if (index == kAdded)
{
update(EPOLL_CTL_DEL, channel);
}
channel->set_index(kNew);
``` 


