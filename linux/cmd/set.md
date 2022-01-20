## 扩展

### 1.在脚本中使用shell

> set 命令的选项之一是双破折线（--）, 将命令行参数替换成 shell 中的各种变量，需要配合shell使用

```
set -- $(getopt -q ab:cd "$@")
```