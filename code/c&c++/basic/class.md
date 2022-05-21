## 初始化带参数的对象

1. 定义成员变量 `A a;`
2. 在构造函数末尾(不是在构造函数体内)，给对象传入参数。
   ```c++
   class A
   {
    public:
       A(int count) { count_ = count }
    
    private:
        int count_;
   };

   class B
   {
    public:
       B() : a(1)
       { }

    private:
        A a;
   };
   ```

## 头文件中定义const常量和static变量
> 引用头文件相当于将头文件的所有内容全部拷贝到源文件中。 

**c**:
1. 在头文件中定义static，可以编译通过，相当于在每个源文件中定义一个只存在于当前文件中的静态变量。
2. 在头文件中定义const， 编译不通过， 会显示重复定义。

**c++**:

**全局变量**:
1. 头文件中的数组一定要加const, 不然会显示重定义。
2. 头文件中定义static, 和 c 情况一致。
3. 头文件中定义const, 可以编译通过，但是会重复构造，造成不必要的内存浪费。

**类中**:
1. 类中使用`const int size = 200; int array[size];` 是错误的，因为对象未创建时，不知道const 是什么, 所以 array 会报错\
   所以需要使用 `enum {SIZE = 200}; int array[size]`的方法来表示常量，但是enum默认是int, 能表示的数据空间有限。
2. const 数据成员的初始化能且只能在构造函数中进行。
3. 可以使用 `static const int size = 200`, static 表明在内存中只有一份，被整个类共享，且存储在全局数据区， const 表明它是一个常量，值不可以改变。 
   
    3.1. 只有`const` 整型变量(整型包括 `bool`、 `char`、 `wchar_t`)和枚举类型可以这么定义，其他的包括 `int 数组`和其他的字符串都只能在类外定义。 \
    3.2. 在类中借助枚举巧妙定义 整形数组。
    ```c++
    class A 
    {
        static const int a = 3;
        enum { arrsize = 2 };

        static const int c[arrsize] = { 1, 2 };

    };
    ```
    3.3. 只允许整型在类中定义的原因是，整型是明确的值，编译器知道它不会被修改。 \
    3.4. 可以使用inline, 它会被翻译成另一个代码段，


**总结**:
1. 不要在头文件中定义const 或 static 常量， 定义在源文件，头文件中使用 extern 引用。
2. 类中定义的全局变量，需要加上static, 表示在内存中只有一份。
