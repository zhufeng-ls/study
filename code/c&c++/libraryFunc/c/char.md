### strncpy

```
char *strncpy(char *dest, const char *src, size_t n)
```

可以先用memset 批量将src 置 0 ，然后在将其拷贝到 dest中。

也就是说，strncpy 拷贝时并不会对字符进行是否为 '\0' 的判断。



### strrchr

```
const char* strrchr (const char *__s, int __c)
```

返回: 返回最后一次出现`c`的位置

### strtoul 和 strtoll

**strtoul**：将str转换成无符号长整形，其他类似strtol（）
```
unsigned long strtoul (const char* str, char** endptr, int base)
```

**strtoll**：与strtol类似，不同的是将str转换成long long int类型的数据
```
long long int strtoll（const char *str, char** endptr, int base）
```

### 字符串相减

字符串指针相减得到的是他们指向的字符串之间的距离。

例如:

```
char* srr = "heelllloo";
char* p = src + 3;

int ret  = p - src; // ret = 3
```