## 取类型的最大值和最小值
=======
## queue
1. queue可以先`pop`元素，再销毁元素。

## other
### 取类型的最大值和最小值

```
std::numeric_limits<uint32_t>::max();
std::numeric_limits<uint32_t>::min();
```
## deque
双端队列，可以在头部和尾部删除元素。


## std::ref

取变量的引用。

主要用途在 函数式编程(如 std::bind)在使用时， 主要是对参数进行拷贝，而不是引用。

## std::swap

交换两个同类型对象的值。
