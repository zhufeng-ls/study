# template

编译时的类型检查，检查 To 是否为 From 的子类型，会使在运行时没有任何关于类型检查的消费。

```c++
if (false)
{
    implicit_cast<From*, To*>(0);
}
```