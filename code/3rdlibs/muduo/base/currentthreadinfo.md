# 当前线程信息

* 用来获取当前线程的真实tid 
* 获取当前的线程名 
* 判断当前线程是否为主线程
* 异常捕获时，打印堆栈信息。

## 实现

**线程 tid**

```c
::syscall(SYS_gettid)
```

__thread: 线程内唯一，确保了tid只能被单线程修改。

**线程名称**
```c
::prctl(PR_SET_NAME, "main");
```

**打印异常堆栈**
```c
backtrace
backtrace_symbols
abi::__cxa_demangle
```

## 优点
1. __builtin_expect, 使用它来告诉编译器最有可能被执行的分支。