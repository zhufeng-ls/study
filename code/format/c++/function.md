# 函数

## 定义
1. 函数的参数排列，依次是: 源对象， 源对象尺寸， 目标对象。

## 使用
1. 当函数参数过长的时候， 可以对参数换行， 第二行的参数从上一行的 `(` 后一位开始。
   ```
   muduo::Thread t2(std::bind(threadFunc2, 42),
                    "thread for free function with argument");
   ```
2. 