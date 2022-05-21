# 线程单例数据模板类

## 实现

### 1. 首先创建一个比较正常的懒汉单例模式

### 2. 创建一个私有的内部类

内部使用 pthread_key_create 来管理外部类的传入对象的生命周期，主要是利用其销毁对象的函数, 即 RAII 技法的使用。

### 3. 创建一个内部类的静态对象

这样内部类的对象释放时，也会在其析构函数内销毁外部类的对象。

这样就完成所有的操作了


模板类静态数据成员的创建
```c++
typename ThreadLocalSingleton<T>::Deleter ThreadLocalSingleton<T>::deleter_;
```
