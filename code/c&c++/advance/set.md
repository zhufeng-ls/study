# set

有序集合

## lower_bound

在集合中找 val, 若找不到，则返回刚好大于 val 的值的迭代器。
```c++
iterator lower_bound (const value_type& val);
```