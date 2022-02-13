# gcc
> gcc 内建函数

## __builtin_expect

> 函数__builtin_expect()是GCC v2.96版本引入的，作用是允许程序员将最有可能执行的分支告诉编译器,以及来减少分支判断时JMP指令跳转浪费的时间。

### 原理

if else 句型编译后, 一个分支的汇编代码紧随前面的代码,而另一个分支的汇编代码需要使用JMP指令才能访问到，
很明显通过JMP访问需要更多的时间, 在复杂的程序中,有很多的if else句型,又或者是一个有if else句型的库函数，每秒钟被调用几万次，
通常程序员在分支预测方面做得很糟糕, 编译器又不能精准的预测每一个分支,这时JMP产生的时间浪费就会很大。

### 函数声明
```
long __builtin_expect(long exp, long c);
```

### 参数说明
* **exp**: 为一个整形表达式，例如(ptr != NULL)
* **c**: 为一个编译期常量，不能使用变量

### 用法
与关键字 if 一起使用，`if (value)` 等价于 `if (__builtin_expert(value, x))`, 与 x 的值无关， x只是判断将哪个分支紧随前面的代码。

**例1**： 

期望 `(x > 0)` 不成立，即期望 `x == 0`,所以执行`else`分支的可能性大,执行`func()`的可能性小 
```
if (__builtin_expect(x, 0))
{
    func();
}
else
{
    //do someting
}
```

**例2**：

期望 `ptr != NULL` 成立，所以执行`func()`的可能性小。
```
if (__builtin_expect(ptr != NULL, 1))
{   
    //do something
}
else
{
    func();
}
```
### 扩展
linux内核、Glib中，我们常常能看到基于__builtin_expect()实现的宏
```
#define likely(x)      __builtin_expect(!!(x), 1)
#define unlikely(x)    __builtin_expect(!!(x), 0)
```