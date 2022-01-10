##  同步网络时间

设置系统时间和网络时间同步

```
$ ntpdate cn.pool.ntp.org
```
可以选择同步阿里时间:
```
$ ntpdate ntp.aliyun.com
```


## 同步硬件时间

将系统时间同步到硬件时间，不然开机时间会重置

```
$ sudo hwclock --systohc
```

## 查看系统时间

```
$ sudo hwclock -r
```

