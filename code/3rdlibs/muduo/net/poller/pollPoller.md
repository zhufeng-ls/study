# PollPoller

## 实现

* ::poll(&*pollfds_.begin(), pollfds_.size(), timeoutMs);
  * 监听事件，和 epoll 不同的是他只能存储 fd 的集合，并没有 data 指针来存储对象。
  * 当poll返回时，该函数会获取当前时间戳，作为函数返回值使用。如果poll返回为0，则说明poll超时但没有发生任何事件；如果poll为负值，则说明poll系统调用失败；如果poll正常返回一个整数，则说明当前有文件描述符活动，需要获取这些文件描述符对应的Channel，并返回给EventLoop，这里使用了fillActiveChannels来获取这些活跃的通道：



因为 epoll 是事件驱动型的，所以可以传入 fd 和 callback 等信息，而 poll 是遍历fd集合的，所以 poll 只能自己维护 fd 集合的容器。


### 添加事件

1. 使用 `channel->index();` 保存在容器中的索引，好直接使用下标获取元素，而 EpollPoller 使用 index() 来保存事件的操作状态，如 Del, Add,New。
2. 在本地使用 channelMap 和 pollfds_ 分别来保存管道和fd。

### 删除事件

从 vector 容器中删除元素，使用该函数之前一般需要调用 updateChannel 函数设置不再关注该Channel对应的文件描述符上的事件。

1. 将当前的元素和末尾的元素交换，再删除末尾的元素。
2. 和末尾的元素交换时，使用的是 std::iter_swap
3. 同时会判断末尾的元素的 fd 是否小于0，若小于0，则 `-fd - 1`。
4. 当要将 channel 的状态置为删除的时候， 会将 pollfd->fd 置为负数，即 `-fd - 1`, 这样 ::poll 函数就不会监听这个事件。

### iter_swap

用来交换两个容器的迭代器指向的元素，和 std::swap 类似，不同的是传入的参数是迭代器，且可以是不同容器的迭代器

## 注意

到这里PollPoller就分析完了，需要注意的是Channel的index_，在PollPoller中，如果index_为-1，则说明该Channel是新的需要加入的通道；如果index_>0，则说明该Channel已经和PollPoller关联了，index_的值用于在pollfds_数组中查找文件描述符对应的pollfd如果index_为其他负值，则说明该文件描述符将不被关注，该Channel也将被移除。