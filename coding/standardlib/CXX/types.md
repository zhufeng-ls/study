### strtoul 和 strtoll

```
strtoul： 
unsigned long strtoul (const char* str, char** endptr, int base)
功能：将str转换成无符号长整形，其他类似strtol（）

strtoll:
long long int strtoll（const char *str, char** endptr, int base）
功能：与strtol类似，不同的是将str转换成long long int类型的数据
```