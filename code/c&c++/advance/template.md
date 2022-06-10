# template

编译时的类型检查，检查 To 是否为 From 的子类型，会使在运行时没有任何关于类型检查的消费。

```c++
if (false)
{
    implicit_cast<From*, To*>(0);
}
```

## 模板特化

不同于模板的实例化，模板参数在某特定类型下的特殊实现叫做模板特化。有时也称做模板的具体化。

demo:
```c++
#include <iostream>
using namespace std;

template<typename T> T Max(T t1,T t2) {
	return (t1>t2)?t1:t2;
}

typedef const char* CCP;
template<> CCP Max<CCP>(CCP s1,CCP s2) {
	return (strcmp(s1,s2)>0)?s1:s2;
}

int main() {
	// 隐式调用实例：int Max<int>(int,int)
	int i=Max(10,5);
	
	// 显式调用特化版本：const char* Max<const char*>(const char*,const char*)
	const char* p=Max<const char*>("very","good");
	cout<<"i:"<<i<<endl;
	cout<<"p:"<<p<<endl;
}
```

写法:
```c++
template<> T Max<T>(T s1, T s2) {
    reutrn T();
}
```

## 模板偏特化

模板特化是对全部参数特化，而模板偏特化是只对部分参数特化。

**只有类模板支持偏特化，函数模板不支持偏特化**

```c++
// 类模板
template<typename T, class N> class TestClass {
public:
	static bool comp(T num1, N num2) {
		cout <<"standard class template"<< endl;
		return (num1<num2) ? true : false;
	}
};

// 对部分模板参数进行特化
template<class N> class TestClass<int, N> {
public:
	static bool comp(int num1, N num2) {
		cout << "partitial specialization" << endl;
		return (num1<num2) ? true : false;
	}
};

// 将模板参数特化为指针
template<typename T, class N> class TestClass<T*, N*> {
public:
	static bool comp(T* num1, N* num2) {
		cout << "new partitial specialization" << endl;
		return (*num1<*num2) ? true : false;
	}
};
```