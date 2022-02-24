## apt 错误

Ubuntu 中的错误消息可能类似于以下内容：

* /var/lib/dpkg/lock
* /var/lib/dpkg/lock-frontend
* /var/lib/apt/lists/lock
* /var/cache/apt/archives/lock

解决方案: 
```
sudo rm /var/lib/dpkg/lock
sudo rm /var/lib/apt/lists/lock
sudo rm /var/cache/apt/archives/lock
```