## 查看端口

使用`lsof`, 只能看到当前用户使用的端口，若要看全局需要加`sudo`权限。
```
$ lsof -i:xxxx
```

## 查看容器内运行的进程

```
$ docker top <container-id | container-name>
```

## 查看容器的底层信息

```
$ docker inspect <container-id | container-name>
```

## 查询最后一次创建的容器

```
$ docker ps -l
```

