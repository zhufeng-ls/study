# 问题
> 编程过程中遇到的问题

* ~~[第三方库](./3rdlibs/issues.md)~~
* ~~[c、c++](./c&c++/issues.md)~~
* ~~[cmake](./cmake/issues.md)~~
* ~~[代码格式](./format/issues.md)~~
* ~~[go](./go/issues.md)~~
* ~~[qt](./qt/issues.md)~~
* ~~[ros](./ros/issues.md)~~
* ~~[sql](./sql/issues.md)~~


迭代器的底层原理？我只记得迭代器是个模仿指针的模板类

C  map的底层原理

红黑树的底层原理

C 语言添加日志功能

对 protobuf 的理解

### 自己写个日志库

SOCK_STREAM（常用）字节流套接字
SOCK_DGRAM 数据报套接字
SOCK_SEQPACKET 有序分组套接字
SOCK_RAW 原始套接字



堆排序的原理是从底部开始上浮，大根堆顶层大于两个子节点，

使用递归的方法，依次对 0 ~ n-1 个节点进行排序



当需要在全局范围内使用一个常量字符串时，是定义宏比较好，还是定义一个常量并将其放在特定的命名空间里面，若是定义宏，是将宏放在共有的config.h 或者 types.h ，还是将其放在当前头文件或源文件中。



malloc , , alloc的区别



#### 脚本练习

写一个脚本，为boost库的每一个库都创建一个软链接



写一个脚本，递归扫描某个目录下的所有的.so库，找到其同目录下是否有以后缀.so结尾的库（例如 安装boost时，就没有生成.so库，需要我们自己手动软链接），没有的话为其创建软链接。



原子操作和锁的不同



xterm 命令 是干嘛的， 为什么使用gdb 调试 ros 需要用到 xterm



使用lseek 移动文件下标后，下次文件打开时，是否在上次下标的位置。



从循环中或者判断中退出函数时，还需要执行几步，从系统层面来讲解。



拥有随机数的函数属于可重入函数吗



write 函数 O_CREATE 和 O_WDONLY 联合使用创建出来的文件是否有读权限，



如何在linux 内核文件中找到 write 的源代码


条件锁和普通锁有什么不同


红黑树 和 hashmap的优缺点， 红黑树查找，删除，增加节点的复杂度。要理解完全红黑树。

apt 和 apt-get 有什么不同

rpc 原理。

### docker的原理

### 什么是明文

### 使用命令为 boost 库创建软链接。



linux ,windows 下文件编码， 以及linux 和 windows显示中文分别用什么编码

当碰到问题时要仔细的思考错误信息的含义，在从网上寻找答案，不能对google形成依赖

性子慢一点，说话慢一点，不要着急的展现工作量和工作能力，做的稳才算成功

当在类的成员函数后加 const 修饰符时，返回数据成员的引用会失败.(原因是加const 相当于在函数内将数据成员限制成了常量， 违反了常量不能有引用的原则)

如何准确、方便的对自己的代码进行测试，以及测试用例编写的原则是什么。

匿名结构体在内存中是怎么处理的了

vector 经常出问题。

释放处于time_wait状态的端口.

new 申请的空间由分配者还是调用者释放。

使用auto接收 std::shared的指针时，引用记数会加1吗。

大根堆和小根堆的原理

nullptr_t 和 nullptr 的不同

std::array 和 数组有什么不同

std::realloc 和 malloc 的不同

cmake 中 enable_testing()

ctest

tcmalloc

搭建交叉编译环境(好像只能搭建arm和x86的环境，没办法搭建ubuntu和windows的环境)


std::shared_ptr 和 std::weak_ptr 的不同，

以及它们如何构成叠加应用?