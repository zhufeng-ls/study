# 代码调试

## assert

对于哪些偶发性情况，可以使用 assert 来进行简单的断言，失败则终止程序，但是会对程序效率有很大的影响。

assert 断言在 windows 下仅 debug 模式有效，而在 linux 下两种模式均有效

### linux 下禁用 assert

1. 在头文件前定义宏 NDEBUG
2. 增加编译器参数 -DNDEBUG