# map

## 常规用法

**遍历**:
```c++
map<int,int> m;
for (auto it = m.begin(); it != m.end(); ++it)
{
    cout << it->first <<  '\t' << it->second << endl;
}
```

## 删除元素
```c++
map<int,int> m;
m.erase(1); // 表示删除key为1的键值对
```