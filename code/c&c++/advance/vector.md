## 移动构造函数

移动构造函数是浅拷贝，std::move,拷贝构造函数是深拷贝
vector中,push_back()需要先创建一个临时对象，然后用拷贝构造函数添加进去，在进行释放，
emplace_back 直接调用构造函数，不需要调用拷贝构造函数。

## 拷贝 vector 里面的内容
1. 取下标0的地址，配合 memcpy拷贝.
   ```
   vector<uint8_t> vec;
   uint8_t* src = &vec[0]
   memcpy(dest, src, vec.size()); 
   ```
2. 使用data函数，获取到数据的起始地址, `vec.data()`。
3. 使用 copy 配合 迭代器使用。
   ```
   copy(arr.begin(), arr.end(), std::ostream_iterator<uint8_t>(dest, "; "));
   ```

## 初始化
申请n个空间，并初始化为0。
```
vector<int> vec(n,0);
```

## 注意
1. vector.size()的返回值是unsighed long 类型，当用户for循环的参数时，一定要将其强转成int， 否则容易发生内存越界
