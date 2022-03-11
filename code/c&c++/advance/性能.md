# 提高性能

## 1.减少分支切换的时间
[调用__builtin_expect函数设置后面跟随的分支](../libraryFunc/c/gcc.md#__builtin_expect)


## 2. 减少对象拷贝时浪费的时间

### 移动构造函数

移动构造函数是浅拷贝，std::move,拷贝构造函数是深拷贝
vector中,push_back()需要先创建一个临时对象，然后用拷贝构造函数添加进去，在进行释放，
emplace_back 直接调用构造函数，不需要调用拷贝构造函数。