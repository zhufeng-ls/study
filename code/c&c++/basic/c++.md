## 虚析构函数

> 当用基类指针指向子类对象指针时，释放析构函数，只会调用基类自身的析构函数，若将其定义为虚析构函数，则先会调用子类的析构函数，再调用子类的析构函数。

## explict
  
 explict可以避免不合时宜的类型转换，CxString  string1(24);和CxString string2 = 10;都可以，是因为当类的构造函数只有一个参数时，编译时会有一个缺省的转换操作，将构造函数对应类型的数据转换成该类的对象。

 下面若不声明 explict ，则编译通过，因为他生成了 CxString(24)的对象，并通过拷贝构造函数生成了 string1。
 
 若声明 explict 则第二个会报错，

 ```
 CxString string1(24);
 CxString string1 = 24;
 ```

 ## override

确认覆盖基类的虚函数，

有三种作用：

1. 做注释用，方便代码阅读
2. 验证是否重写了父类的虚函数，若不为虚函数则报错

## inline

将函数翻译成代码段，避免发生函数跳转，提高运行速度。

### 注意

任何函数都可以声明内联，因为类的成员函数也只是传入了this指针的普通函数，他们在内存中的存储位置都是一样的，所以编译器并不会对他们做差别处理。

声明了内联的函数不一定内联，即内联只是对编译器的一种建议。可以将该关键字作为意图的表达，而不用在意其是否内联。

静态函数也可以内联，但其是否内联生效则值得商榷。

## 友元类和友元函数

参考网址:

http://c.biancheng.net/view/169.html

### 友元类

在类 A 中声明 B 为友元类，则 B 中可以访问 A 的所有成员。

写法如下:
```c++
friend  class  类名;
```

demo:
```c++
class CCar
{
private:
    int price;
    friend class CDriver;  //声明 CDriver 为友元类
};
class CDriver
{
public:
    CCar myCar;
    void ModifyCar()  //改装汽车
    {
        myCar.price += 1000;  //因CDriver是CCar的友元类，故此处可以访问其私有成员
    }
};
int main()
{
    return 0;
}
```

### 友元函数

在定义一个类的时候，可以把一些函数（包括全局函数和其他类的成员函数）声明为“友元”，这样那些函数就成为该类的友元函数，在友元函数内部就可以访问该类对象的私有成员了。

demo:
```c++
#include<iostream>
using namespace std;
class CCar;  //提前声明CCar类，以便后面的CDriver类使用
class CDriver
{
public:
    void ModifyCar(CCar* pCar);  //改装汽车
};
class CCar
{
private:
    int price;
    friend int MostExpensiveCar(CCar cars[], int total);  //声明友元
    friend void CDriver::ModifyCar(CCar* pCar);  //声明友元
};
void CDriver::ModifyCar(CCar* pCar)
{
    pCar->price += 1000;  //汽车改装后价值增加
}
int MostExpensiveCar(CCar cars[], int total)  //求最贵气车的价格
{
    int tmpMax = -1;
    for (int i = 0; i<total; ++i)
        if (cars[i].price > tmpMax)
            tmpMax = cars[i].price;
    return tmpMax;
}
int main()
{
    return 0;
}
```


