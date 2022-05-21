# 时间

## 日期

### 1. 格式化输出
```c
char timebuf[32];
struct tm tm;
*now = time(NULL);
gmtime_r(now, &tm); // FIXME: localtime_r ?
strftime(timebuf, sizeof timebuf, ".%Y%m%d-%H%M%S.", &tm);
```