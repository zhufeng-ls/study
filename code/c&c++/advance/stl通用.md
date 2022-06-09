## reverse

反转
```c++
reverse(s.begin(), s.end());
```

## search

查找 [first,last)区域（左闭右开）内 [s_first,s_last) 首次出现的位置，若找不到，则返回 last 的位置

```c++
template< class ForwardIt1, class ForwardIt2 >
ForwardIt1 search( ForwardIt1 first, ForwardIt1 last,
                   ForwardIt2 s_first, ForwardIt2 s_last );

```