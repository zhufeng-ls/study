# 堆

## 优先队列
> 也分为大根堆和小根堆。

### 大根堆
**应用**: 滑动窗口求最大元素。 

**写法**:\
```c++
#include <queue>

std::priority_queue<int> queue_;
```

### 小根堆
**应用**: 第k大元素，也就是小根堆大小为k的第一个元素。

**写法**: \
```c++
#include <queue>

std::priority_queue<int,vector<int>,greater<int> >q;
```

**参数**:  <类型,<存储方式>,<比较函数> >