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

## 查找元素

```c++
std::map<string, string>::const_iterator it = headers_.find(field);
if (it != headers_.end())
{
    result = it->second;
}
```

## 交换容器

```c++
void swap(map& __x)
```

## 注意

**`map<int,vector<int>> m`中,  `m[2].push_back(3)`; 会应用到`map`中吗, 当`m[2]`不存在时，可以这样赋值吗
或者是`map<int,vector<int>&> m` ?**

1). m[2] 返回的是引用，所以对其直接进行操作也会修改map的值, 若 `auto m1 = m[2];`, 则m1是拷贝的新的对象，它的修改并不会应用到m[2]中， 所以正确的写法是 `auto& m1 = m[2];`。\
2). 当 m[2]不存在时，可以`m[2] = {1,2,3};` 这样赋值，因为当不存在时，会创建一个带有默认值的pair键值对。