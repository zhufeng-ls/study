# 原子操作

用途： 对于那种不那么频繁，但是比较重要的数据在多线程之间的数据同步，例如统计线程的数量。

写法: 模板类的方式，它没有使用c++ 标准库中的 atomic, 而是使用一些系统API。

例如:
```c
__sync_fetch_and_add
__sync_val_compare_and_swap
__sync_lock_test_and_set
```