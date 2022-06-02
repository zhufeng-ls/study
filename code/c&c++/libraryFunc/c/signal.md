# signal

描述linux中的各种信号

* SIGPIPE: 如果一个 socket 在接收到了 RST packet 之后，程序仍然向这个 socket 写入数据，那么就会产生SIGPIPE信号。
* SIGTERM: 默认的 kill 信号，即 -15 。
* SIGINT: ctrl + c 的信号

## 忽视信号

```c
::signal(SIGPIPE, SIG_IGN);
```

## 监听信号

```c
void exitHandler()
{
    printf("signal here");
}

signal(SIGINT, exitHandler);
```