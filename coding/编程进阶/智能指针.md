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

