# 单例模式

使用 `pthread_once` 来确保单例模式, `pthread_once` 确保函数只会执行一次。

实际"一次性函数"的执行状态有三种：NEVER（0）、IN_PROGRESS（1）、DONE （2），如果once初值设为1，则由于所有pthread_once()都必须等待其中一个激发"已执行一次"信号，因此所有pthread_once ()都会陷入永久的等待中；如果设为2，则表示该函数已执行过一次，从而所有pthread_once()都会立即返回0。

```c++
static void init()
{
    value_ = new T();
    if (!detail::has_no_destroy<T>::value)
    {
        ::atexit(destroy);
    }
}

pthread_once(&ponce_, &Singleton::init);
```

## 如何确保模板类型 T 不为空


```c++
typedef char T_must_be_complete_type[sizeof(T) == 0 ? -1 : 1];
T_must_be_complete_type dummy;
(void)dummy;
```