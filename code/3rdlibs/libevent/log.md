#### __attribute__((noreturn))的用法

表示没有返回值，使用此用法的函数，在被调用后，可以直接退出，而调用它的函数可以不必返回一个值，即用来抑制未达到代码路径的错误。

例如，下面没有返回值会报错

```
$ cat test1.c
extern void exitnow();

int foo(int n)
{
        if ( n > 0 )
	{
                exitnow();
		/* control never reaches this point */
	}
        else
                return 0;
}

$ cc -c -Wall test1.c
test1.c: In function `foo':
test1.c:9: warning: this function may return with or without a value
```



而下面则不会报错。

```
$ cat test2.c
extern void exitnow() __attribute__((noreturn));

int foo(int n)
{
        if ( n > 0 )
                exitnow();
        else
                return 0;
}

$ cc -c -Wall test2.c
no warnings!
```



#### __attribute__((format(printf, a, b)))

可以给被声明的函数加上printf 或者 scanf 的特征， 它可以使编译器检查函数声明和函数实际调用参数之间的格式字符串是否匹配， 

* __attribute__((format(printf, a, b)))
* __attribute__((format(scanf, a, b)))

​    其中 a, b的含义为：

​		a: 第几个参数为格式化字符串

​		b: 参数集合中的第一个，即参数"..." 的第一个参数排在第几。



​	下面给个例子, 需要打开警告 -Wall 输出调试信息。

> ```
> #include <stdio.h>
> #include <stdarg.h>
> 
> #if 1
> #define CHECK_FMT(a, b)	__attribute__((format(printf, a, b)))
> #else
> #define CHECK_FMT(a, b)
> #endif
> 
> void TRACE(const char *fmt, ...) CHECK_FMT(1, 2);
> 
> void TRACE(const char *fmt, ...)
> {
> 	va_list ap;
> 
> 	va_start(ap, fmt);
> 
> 	(void)printf(fmt, ap);
> 
> 	va_end(ap);
> }
> 
> int main(void)
> {
> 	TRACE("iValue = %d\n", 6);
> 	TRACE("iValue = %d\n", "test");
> 
> 	return 0;
> }
> ```

则会输出如下警告

```
main.cpp: In function ‘int main()’:
main.cpp:26:31: warning: format ‘%d’ expects argument of type ‘int’, but argument 2 has type ‘const char*’ [-Wformat=]
  TRACE("iValue = %d\n", "test");
```

如果不使用__attribute__ format则不会有警告。



