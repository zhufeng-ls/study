## Your connection is not private

**问题**： DNS 配置问题

**解决方法**:
1. 打开谷歌浏览器设置(settings)
2. 搜索 `DNS` , 找到 `Security` 目录下的 `Use secure DNS`选项
3. 将配置改为 `OpenDNS`


## 您目前无法访问，因为此网站使用了 HSTS。网络错误和攻击通常是暂时的,因此,此网页稍后可能会恢复

1. 在谷歌浏览器中输入 `chrome://net-internals/#hsts`, 下拉到最后一个。

    ![chrome_HSTS](./images/chrome_1.png)

2. 输入访问异常的网址， 并点击`Delete`删掉访问异常的网址