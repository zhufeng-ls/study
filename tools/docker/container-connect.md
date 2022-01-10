## 指定端口连接

**选项**
* **-P**: 容器内部端口随机映射到主机的高端口
* **-p**: 容器内部端口映射到指定的端口。比如 `-p 5000:5000`,第一个端口号是容器的端口，第二个是主机的端口。

## 创建网络

```
$ docker network create -d bridge test-net
```

**参数说明**
* **-d**: 指定docker 网络类型， 有bridge(桥接), overlay(覆盖网络)。
* **test-net**: 指定网络的名称。

## 容器连接到指定的网络

```
$ docker run -itd --name test1 --network test-net ubuntu /bin/bash
```
## 配置 DNS

在宿主机的 `/etc/docker/daemon.json` 文件中增加以下内容来设置全部容器的DNS。
```
{
    "dns": [
        "114.114.114.114",
        "8.8.8.8"
    ]
}
```

配置完需要重启docker才生效。

查看DNS是否生效可以使用以下命令
```
$ docker run -it --rm -h host_ubuntu --dns=114.114.114.114 --dns-search=test.com ubuntu
```
**参数说明**:
* **--rm**: 容器退出后自动清理容器内部的文件系统
* **-h hostname 或 --hostname=hostname**: 设定容器的主机名
* **--dns=IPADDR**: 添加dns到容器的/etc/resolv.conf中，让容器来解析所有不在/etc/hosts中的主机名。
* **--dns-search=DOMAIN**: 设定容器的搜索域，当设置搜索域为.example.com时，在搜索一个名为host的主机时，不仅搜索host，还会搜索 `host.example.com`

如果在容器启动时没有指定`--dns` 和 `--dns-search`, docker 会默认使用宿主主机上的 /etc/resolv.conf 来配置容器的DNS。