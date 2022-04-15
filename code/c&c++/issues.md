# 问题
* ~~[进阶](./advance/issues.md)~~
* ~~[基础理论](./advance/issues.md)~~
* ~~[c、c++差异](./difference/issues.md)~~
* ~~[c、c++ 库函数](./libraryFunc/issues.md)~~

匿名结构体在内存中是怎么处理的了

什么是虚继承

多态时动态绑定和静态多态

为什么 time_wait的等待时间是2msl

epoll使用calloc申请空间

自旋锁和休眠锁有什么不同

string 的 c_str()的生存周期，是不是调用他的函数或者语句结束，为什么可以作为memcpy的拷贝源

结构体的内存布局

spdlog 怎么将日志级别通过配置文件配置.

使用assert 会浪费大量资源吗，以及是否会在release模式失效，什么时候该决定使用assert

为了避免cpu百分百占用，一般要进行休眠，但是休眠的具体时间应该如何判断了， 建安经常写的是 1ms

内存池和线程池的使用。

memmove

realloc

## 疑问
1. 细粒度锁和粗粒度锁的应用场景？
2. 什么时候用到原子操作？ 比如`count++`这类操作就可以使用原子操作。



#define CAPABILITY(x) \
  __attribute__(capability(x))

#define SCOPED_CAPABILITY \
  __attribute__(scoped_lockable)

#define GUARDED_BY(x) \
  __attribute__(guarded_by(x))


#define PT_GUARDED_BY(x) \
  __attribute__(pt_guarded_by(x))

#define ACQUIRED_BEFORE(...) \
  __attribute__(acquired_before(__VA_ARGS__))

#define ACQUIRED_AFTER(...) \
  __attribute__(acquired_after(__VA_ARGS__))

#define REQUIRES(...) \
  __attribute__(requires_capability(__VA_ARGS__))

#define REQUIRES_SHARED(...) \
  __attribute__(requires_shared_capability(__VA_ARGS__))

#define ACQUIRE(...) \
  __attribute__(acquire_capability(__VA_ARGS__))

#define ACQUIRE_SHARED(...) \
  __attribute__(acquire_shared_capability(__VA_ARGS__))

#define RELEASE(...) \
  __attribute__(release_capability(__VA_ARGS__))

#define RELEASE_SHARED(...) \
  __attribute__(release_shared_capability(__VA_ARGS__))

#define TRY_ACQUIRE(...) \
  __attribute__(try_acquire_capability(__VA_ARGS__))

#define TRY_ACQUIRE_SHARED(...) \
  __attribute__(try_acquire_shared_capability(__VA_ARGS__))

#define EXCLUDES(...) \
  __attribute__(locks_excluded(__VA_ARGS__))

#define ASSERT_CAPABILITY(x) \
  __attribute__(assert_capability(x))

#define ASSERT_SHARED_CAPABILITY(x) \
  __attribute__(assert_shared_capability(x))

#define RETURN_CAPABILITY(x) \
  __attribute__(lock_returned(x))

#define NO_THREAD_SAFETY_ANALYSIS \
  __attribute__(no_thread_safety_analysis)

  __attribute__ ((noinline))


### __attribute__

它是一个编译器指令，指定声明的特征，允许更多的错误检查和高级优化，关键字 `__attribute` 后面跟两组括号（双括号使宏输出更为容易）。括号内是逗号分隔的属性列表。

`__attribute__` 指令放在函数、变量和类型声明之后。


### __attribute__(guarded_by(mutex))

是为了保护线程安全，使用该属性后，线程要使用相应变量，必须先锁定mutex



###  __typeof__(var) 是gcc对C语言的一个扩展保留字，用于声明变量类型,var可以是数据类型（int， char*..),也可以是变量表达式。
__typeof__(int *) x; //It   is   equivalent   to  'int  *x';

__typeof__(int) a;//It   is   equivalent   to  'int  a';

__typeof__(*x)  y;//It   is   equivalent   to  'int y';

__typeof__(&a) b;//It   is   equivalent   to  'int  b';

__typeof__(__typeof__(int *)[4])   z; //It   is   equivalent   to  'int  *z[4]';