## initializer_list
```c++
initializer_list是某种类型的数组，但是内部数据都是const T类型，可以整体作为参数传递，由{}进行初始化。

initializer_list对象只能用大括号{}初始化。

拷贝一个initializer_list对象并不会拷贝里面的元素。其实只是引用而已。而且里面的元素全部都是const的。

///> 在c++11之前，max函数的源程序是这样的,只能比较两者之间的大小
template <class T> const T& max (const T& a, const T& b);
template <class T, class Compare>
  const T& max (const T& a, const T& b, Compare comp);  //支持自己编写的比较函数
  
///>但是有了initializer_list后，c++11标准库中添加另外一种实现方式max函数可以传递更多的参数
template <class T> T max (initializer_list<T> il);
template <class T, class Compare>
  T max (initializer_list<T> il, Compare comp);
  
///>例子
cout << max({ 54,16,48,5 }) << endl;
```

