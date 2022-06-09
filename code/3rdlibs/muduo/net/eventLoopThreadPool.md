# EventLoopThreadPool

事件循环线程池。

原理是创建多个 EventLoopThread 对象， 通过一个 vector 来管理事件循环，通过 getNextLoop 获取到想要的事件循环对象。

## 细节

使用 std::uniqued_ptr 来管理 EventLoopThread 对象，并通过 vector<std::uniqued_ptr<EventLoopThread>> 来存储线程对象，这样 eventLoopThreadPoll 对象退出后， 智能指针也会自动是否，而不用担心他们的生命周期了。

而 loop 对象其实是线程中局部对象的指针，当线程退出后，指针也会失效了。
