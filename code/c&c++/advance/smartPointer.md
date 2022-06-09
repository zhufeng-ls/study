# 智能指针

## shared_ptr

```
// 先构建 Base(1), 引用计数为 1.
shared_ptr<Base> ptr(new Base(1));
// 先构建 Base(1), 再减去引用计数，因此 Base(1)被销毁. 
// Base(2) 引用为 1.
ptr.reset(new Base(2));
// Base(2) 引用计数减一， 因此被销毁。
ptr.reset();
```

## unique_str

unique_ptr 不共享它的指针。它无法复制到其他 unique_ptr，无法通过值传递到函数，也无法用于需要副本的任何标准模板库 (STL) 算法。只能移动unique_ptr。这意味着，内存资源所有权将转移到另一 unique_ptr，并且原始 unique_ptr 不再拥有此资源。我们建议你将对象限制为由一个所有者所有，因为多个所有权会使程序逻辑变得复杂。因此，当需要智能指针用于纯 C++ 对象时，可使用 unique_ptr，而当构造 unique_ptr 时，可使用 `make_unique` 函数。

std::unique_ptr实现了独享所有权的语义。一个非空的std::unique_ptr总是拥有它所指向的资源。转移一个std::unique_ptr将会把所有权也从源指针转移给目标指针（源指针被置空）。拷贝一个std::unique_ptr将不被允许，因为如果你拷贝一个std::unique_ptr,那么拷贝结束后，这两个std::unique_ptr都会指向相同的资源，它们都认为自己拥有这块资源（所以都会企图释放）。因此std::unique_ptr是一个仅能移动（move_only）的类型。当指针析构时，它所拥有的资源也被销毁。默认情况下，资源的析构是伴随着调用std::unique_ptr内部的原始指针的delete操作的。

**使用**

unique_ptr 不能使用拷贝构造，也不能使用重载运算符 `=` 赋值，但是有个例外，它可以作为函数返回的参数，可以使用另一个 `std::unique_ptr` 接收函数返回的 `std::unique_ptr`。

如果嫌弃 std::unique_ptr<T> 的名称太长，又不想销毁以前的对象，可以使用 ‵std::unique<T>::get()‵ 获取 new 的对象，直接对源对象进行操作。

## 扩展

1. 当函数参数为智能指针时，可以直接传入 new 生成的对象，它会自动转变成智能指针。

## 单例模式

1. 单例模式中，初始化智能指针时，使用 `std::shared<T>(new T)`， 不能使用 std::make_shared<T>(), 后者会调用构造函数， 因为构造函数是私有的，所以会初始化失败。


## static_pointer_cast

基类智能指针向派生类智能指针的转换，且参数必须为 std::shared\<T\>, 

## 接收任何类型的智能指针

```c++
void Channel::tie(const std::shared_ptr<void>& obj)
```

## reinterpret_cast

从内存的角度对指针的类型进行转换， 转换前后的类型可以没有任何关联

随意获取一个类型的指针，而不用创建这个类型的对象。
```c++
reinterpret_cast<Timer*>(UINTPTR_MAX)
```

## shared_from_this

stl 支持的函数，表示将 this 转变为 std::shared_ptr 指针，在类的内部使用，通常用于需要接收 std::shared_ptr 的函数。

## std::weak_ptr

### std::weak_ptr<T>::lock

创建一个新的 std::shared_ptr 指针，用来分享对象的管理权。

如果没有管理的对象的话，返回的 std::shared_ptr 也为空。
