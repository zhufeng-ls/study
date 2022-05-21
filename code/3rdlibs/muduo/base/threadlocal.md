# 线程本地数据

和 `__thread` 不同的是, ThreadLocal 类可以存储复杂的数据，而 __thread 只能存储简单的数据类型

## 实现

pthread_key_create 只需要在主线程中调用一次，之后在剩下的线程中会自动判断是否key是否绑定了数据，若没有绑定，则创建新的对象并绑定。

```c++
pthread_key_create(&pkey_, &ThreadLocal::destructor); // 声明数据的销毁方式， 第二个参数设置为 NULL 则使用默认清理函数
pthread_key_delete(pkey_); // 销毁key
pthread_setspecific(pkey_, newObj); // 设置key值对应的数据
pthread_getspecific(pkey_); //获取key值对应的数据
```

> 模板类对象的销毁，可以使用模板类的静态成员函数，因为模板类的成员函数，可以识别类的真实类型。