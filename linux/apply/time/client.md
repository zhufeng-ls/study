# 客户端时间同步

参考网址:

https://ubuntu.com/server/docs/network-ntp

## timedatectl

建议使用的方式，和 ntp 会有冲突，不建议使用ntp。

问题:

存在开机时服务不会自启动的问题，需要输入管理员密码激活才会启动