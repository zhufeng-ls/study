# 时间戳

时间戳的格式化字符串，整数值，当前字符串，时间差值，时间比较

## 实现
* PRId64： 使用它在不同的平台上输出不同类型的整数。\
    使用它需要包含 __STDC_FORMAT_MACROS 宏
    ```c
    #ifndef __STDC_FORMAT_MACROS
    #define __STDC_FORMAT_MACROS
    #endif
    ```
* gettimeofday: 或者当前时间