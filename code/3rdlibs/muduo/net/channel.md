# channel 

是处理需要监听的文件描述符的事件的，比如读事件，写事件，关闭事件。

根据需要监听的事件，传入对应的回调函数。

## 入参

* loop: 绑定的事件循环
* fd: 监听的文件描述符

## 实现

使用 events_ 和 revents_ 来标记关注的事件和实际发生的事件，该方法和 struct pollfd 结构体类似。
