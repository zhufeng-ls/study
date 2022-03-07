# 错误
linux 通过errno获取发生的错误。使用 strerror 和 strerror_r可以将错误字符串化。

## strerror_r
更加安装的转换错误的方式，因为它是带缓存的
```
#include <string.h>

int strerror_r(int errnum, char *buf, size_t n);
```

## strerror
```
#include <string.h>

char *strerror(int errnum);
```