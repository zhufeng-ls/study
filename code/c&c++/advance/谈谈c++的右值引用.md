# 谈谈 C++ 中的右值引用

参考网址:

https://liam.page/2016/12/11/rvalue-reference-in-Cpp/

https://www.cnblogs.com/shadow-lr/p/Introduce_Std-move.html

**右值引用主要是实现所有权的转移，而不是位置的转移，即对象在内存中的位置并没有改变，新对象指向原来的位置，原来的对象则置为空指针**

## 左值 和 右值

在 C  语言中的解释中。
* 左值是可以位于赋值运算符 = 左侧的表达式（当然，左值也可以位于 = 的右侧），而
* 右值是不可以位于赋值运算符 = 左侧的表达式。

在 C++ 标准中。

一个表达式是左值还是右值，取决于我们使用的是它的值还是它在内存中的位置（作为对象的身份）。也就是说一个表达式具体是左值还是右值，要根据实际在语句中的含义来确定。
例如:
```c++
int foo(42);
int bar;

// 将 foo 的值赋给 bar，保存在 bar 对应的内存中
// foo 在这里作为表达式是右值；bar 在这里作为表达式是左值
// 但是 foo 作为对象，既可以充当左值又可以充当右值
bar = foo;
```

因为 C++ 中的对象本身可以是一个表达式，所以这里有一个重要的原则，即

* 在大多数情况下，需要右值的地方可以用左值来替代，但需要左值的地方，一定不能用右值来替代。

又有一个重要的特点，即
* 左值存放在对象中，有持久的状态；
* 右值要么是字面常量，要么是在表达式求值过程中创建的临时对象，没有持久的状态。

扩展理解：
> 左值是表达式结束后依然存在的持久对象(代表一个在内存中占有确定位置的对象)
> 
> 右值是表达式结束时不再存在的临时对象(不在内存中占有确定位置的表达式）

便携方法：对表达式取地址，如果能，则为左值，否则为右值



## 左值引用和右值引用
左值引用是常见的引用，所以一般在提到「对象的引用」的时候，指得就是左值引用。如果我们将一个对象的内存空间绑定到另一个变量上，那么这个变量就是左值引用。

另一方面，由于常量左值引用保证了我们不能通过引用改变对应内存空间的值，因此我们可以将右值绑定在常量引用上。
```c++
int foo(42);
int& bar = foo;  // OK: foo 在此是左值，将它的内存空间与 bar 绑定在一起
int& baz = 42;   // Err: 42 是右值，不能将它绑定在左值引用上
const int& qux = 42;  // OK: 42 是右值，但是编译器可以为它开辟一块内存空间，绑定在 qux 上
```

右值引用也是引用，但是它只能且必须绑定在右值上，右值只能为临时对象或者字面常量。
```c++
int foo(42);
int& bar = foo;        // OK: 将 foo 绑定在左值引用上
int&& baz = foo;       // Err: foo 可以是左值，所以不能将它绑定在右值引用上
int&& qux = 42;        // OK: 将右值 42 绑定在右值引用上
int&& quux = foo * 1;  // OK: foo * 1 的结果是一个右值，将它绑定在右值引用上
int& garply = foo++;   // Err: 后置自增运算符返回的是右值，不能将它绑定在左值引用上
int&& waldo = foo--;   // OK: 后置自减运算符返回的是右值，将它绑定在右值引用上

```

由于右值引用只能绑定在右值上，而右值要么是字面常量，要么是临时对象，所以：
* 右值引用的对象，是临时的，即将被销毁
* 并且右值引用的对象，不会在其它地方使用。
* 当函数参数为右值引用的时候，调用函数时，若传入的参数不是右值引用，则会报错。

> 敲黑板：这是重点！

这两个特性意味着：接受和使用右值引用的代码，可以自由地接管所引用的对象的资源，而无需担心对其他代码逻辑造成数据破坏。


## 引用叠加

```c++
typedef int& intR;
typedef intR& intRR;

int main() {
    int foo = 42;
    intR bar = foo;
    intRR baz = bar;
    return 0;
}
```
在这里，intR 实际上是 int&。因此 intRR 就变成了 int& &，注意两个 & 之间有一个空格，表示这是对 int 类型引用的引用，也就是引用的叠加。在 C++11 之前，编译这份代码是会报错的：
```
ref_test.cpp:2:15: 错误：无法声明对‘intR {aka int&}’的引用
```

这是因为在 C++11 之前，C++ 标准没有写明引用叠加。在 C++11 中，引用叠加有如下规则：
* T& &变为T&
* T& &&变为T&
* T&& &变为T&
* T&& &&变为T&&

这有点类似布尔代数中的与运算：左值引用是 0，右值引用是 1。因此，在 C++11 中，上述代码中的 intRR 实际就是 int& 类型。这样一来，代码就合法了

同样的引用叠加规则，也可以应用到模板参数推导中。看这个例子
```c++
template <typename T> void func(T&& foo);
auto fp = func<int&&>;
```

在这里，func 是一个模板函数，fp 是函数指针。要确定 fp 的实际类型，就要先确定模板函数参数的类型。

* 在模板中，T 被 int&& 替换，因此 T 是 int 的右值引用；
* 在函数参数列表声明中，foo 是 T&& 类型，因此是 int&& && 类型，根据叠加规则，实际 foo 是 int&& 类型。
  
这样一来，fp 就是 void (*)(int&&) 类型的指针了。

## std::move
* std::move作用主要可以将一个左值转换成右值引用，从而可以调用C++11右值引用的拷贝构造函数
* std::move应该是针对你的对象中有在堆上分配内存这种情况而设置的，如下


用途:

在vector和string这个场景，加个std::move会调用到移动语义函数，避免了深拷贝。

除非设计不允许移动，STL类大都支持移动语义函数，即可移动的。 另外，编译器会默认在用户自定义的class和struct中生成移动语义函数，但前提是用户没有主动定义该类的拷贝构造等函数(具体规则自行百度哈)。 

**因此，可移动对象在<需要拷贝且被拷贝者之后不再被需要>的场景，建议使用std::move触发移动语义，提升性能。**

## 完美转发

```c++
#include <iostream>

using std::cout;
using std::endl;

template<typename T>
void func(T& param) {
    cout << "传入的是左值" << endl;
}

template<typename T>
void func(T&& param) {
    cout << "传入的是右值" << endl;
}

template<typename T>
void warp(T&& param) {
    func(param);
}

int main() {
    int num = 2019;
    warp(num);
    warp(2019);
    return 0;
}
```

输出的结果

```
传入的是左值
传入的是左值
```

和预期的不一样，下面我们来分析一下原因：

warp()函数本身的形参是一个万能引用，即可以接受左值又可以接受右值；第一个warp()函数调用实参是左值，所以，warp()函数中调用func()中传入的参数也应该是左值；第二个warp()函数调用实参是右值，根据上面所说的引用折叠规则，warp()函数接收的参数类型是右值引用，那么为什么却调用了调用func()的左值版本了呢？这是因为在warp()函数内部，右值引用类型变为了左值，因为参数有了名称，我们也通过变量名取得变量地址

那么问题来了，怎么保持函数调用过程中，变量类型的不变呢？这就是我们所谓的“变量转发”技术，在C++11中通过std::forward()函数来实现。我们来修改我们的warp()函数如下：

```c++
template<typename T>
void warp(T&& param) {
    func(std::forward<T>(param));
}
```

这样当 param 是左值时，std::forward() 返回的是左值， param 为右值时，返回的是右值，详情可见 `引用叠加`


## 用途
1. 接管资源
2. 转移所有权
3. 避免拷贝
