# 日志文件

## 1. 实现

### 1.1 构造函数
参数: 

* basename: 基础日志名称
* rollSize: 日志文件的大小
* threadSafe: 是否线程安全
* flushInterval: 刷新流的间隔
* checkEveryN:  每推送 N 次后检查是否需要回滚日志文件。

### 1.2 append

> 添加数据流到文件中

使用的是 `fwrite_unlocked`, 是否线程安全是通过此类中的锁来实现的，而不是使用 fwrite。

> 疑问? 外部加锁 + fwrite_unlocked 真的比 fwrite 效率高吗？

### 1.3 flush 

内部使用 `fflush()` 来实现

### 1.4 rollFile

获取日志名称，关闭旧的日志文件，打开新的日志文件。
```c++
file_.reset(new FileUtil::AppendFile(filename));
```

### 1.5 append_unlocked

函数原型:
```c++
append_unlocked(const char* logline, int len);
```

步骤:
1. 当目前写的字数大于 rollSize_ 时，回滚日志，否则进行下一步判断。
2. 若添加日志的次数小于 `checkEveryN_`, 则跳过
3. 若添加日志的次数大于 `checkEveryN_`，则重置添加日志的次数，并判断两者之间的时间是否超过了一个时间间隔
4. 若超过了一个时间间隔，则回滚新的日志文件
5. 若不超，则判断是否超过了刷新时间。
6. 若超过了刷新时间，则刷新（fflush()） 当前的文件流