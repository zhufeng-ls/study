# 初始化带参数的对象

1. 定义成员变量 `A a;`
2. 在构造函数末尾(不是在构造函数体内)，给对象传入参数。
   ```c++
   class A
   {
    public:
       A(int count) { count_ = count }
    
    private:
        int count_;
   };

   class B
   {
    public:
       B() : a(1)
       { }

    private:
        A a;
   };
   ```