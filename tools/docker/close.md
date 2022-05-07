# 关闭容器

使用 `docker stop` 关闭容器


docker stop 做了什么? \
docker stop 命令是在对 detached 模式运行的容器发出停止命令时使用的，从发送信号上来讲，它将发送 SIGTERM 信号给容器，通知其结束运行。

SIGINT 一般用于关闭前台进程，SIGTERM 会要求进程自己正常退出。\
当我们在 shell 中给进程发送 SIGTERM 和 SIGINT 信号的时候，这些进程往往都能正确的处理。但是在 docker 中却不灵了。这是因为在 docker 中，只会将 SIGTERM 等所有的 signal 信号发送给 PID 为 1 的进程，当我们 docker 中运行的进程
的进程号不是 1 时，就不会收到这样的信号。