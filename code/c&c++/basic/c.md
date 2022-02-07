## static 静态成员赋值

静态成员中不允许函数调用， 且静态成员初始化必须用常量赋值。

## 库的原理

.so 是将任意多个.o文件链接在一起，引用头文件时，会在.so文件中寻找该.h文件对应的.o文件，因此可以通过.so和.h文件引用其他库的代码。

## 数据类型

  | C 数据类型      | LP32 | LP64 |
  | --------------- | ---- | ---- |
  | `char`          | 8    | 8    |
  | `short`         | 16   | 16   |
  | `int`           | 32   | 32   |
  | `long`          | 32   | 64   |
  | `long` `long`   | 64   | 64   |
  | `pointer`       | 32   | 64   |
  | `enum`          | 32   | 32   |
  | `float`         | 32   | 32   |
  | `long` `double` | 128  | 128  |

## RTTI

运行时类型识别， 可以在注释后面加上， 表明这条语句是一条类型检查

## main 参数解析

```
argc: 命令行参数个数，至少一个，因为argv[0]为执行程序名。
argv: 命令行参数，argv[0]为可执行程序名。例如，为a.out,其他参数正常排列在后面
```

## 变长结构体

**写法**:
```
struct v_struct { 
    int i; 
    int a[0];
};
```

变长结构体需要在堆上使用`malloc`申请空间，因为栈上的空间是函数参数入栈之前就确定好了的。

**变长结构体和指针的区别**:

1. 变长数组只能放在结构体尾部，而指针可以放在任何地方
2. 变长数组不占内存，它只是一个占位符，而指针会占用内存空间。
3. 变长结构体可以直接释放对象，而指针需要先释放自己，在释放结构体。
4. 变长结构体不能放在 c++ 的类中，因为编译器会将一些额外的信息放在类的最后，例如 vptr 或者虚基类。
