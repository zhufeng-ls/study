# 通用

## 宏函数

### 检查返回值是否为0
```c
#ifdef CHECK_PTHREAD_RETURN_VALUE

#define MCHECK(ret) ({ __typeof__ (ret) errnum = (ret);         \
                       if (__builtin_expect(errnum != 0, 0))    \
                         __assert_perror_fail (errnum, __FILE__, __LINE__, __func__);})

#else  // CHECK_PTHREAD_RETURN_VALUE

// 检查ret 是否为 0
#define MCHECK(ret) ({ __typeof__ (ret) errnum = (ret);         \
                       assert(errnum == 0); (void) errnum;})

#endif // CHECK_PTHREAD_RETURN_VALUE
```

* **__builtin_expect(val, ret)**: ret为0或1，检查val的值是否为ret。
* **__assert_perror_fail**: __assert_perror_fail函数根据errnum值输出相应的诊断信息到stderr标准错误流中，然后调用abort函数终止程序。__assert_perror_fail是调用__strerror_r函数（也就是strerror_r）来输出errnum诊断信息的。