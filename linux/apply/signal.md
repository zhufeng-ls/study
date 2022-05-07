# 捕获信号

```shell
trap 'echo "SIGTERM"; exit' SIGTERM
```

格式:
* trap: 命令名称
* 'echo "SIGTERM"; exit': 执行的操作，中间可以为函数
* SIGTERM: 捕获的信号