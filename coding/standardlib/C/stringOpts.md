### strncpy

```
char *strncpy(char *dest, const char *src, size_t n)
```

可以先用memset 批量将src 置 0 ，然后在将其拷贝到 dest中。

也就是说，strncpy 拷贝时并不会对字符进行是否为 '\0' 的判断。

