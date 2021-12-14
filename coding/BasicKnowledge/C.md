### static 静态成员赋值

静态成员中不允许函数调用， 且静态成员初始化必须用常量赋值。

### 库的原理

.so 是将任意多个.o文件链接在一起，引用头文件时，会在.so文件中寻找该.h文件对应的.o文件，因此可以通过.so和.h文件引用其他库的代码。