# container

**中文:** 容器

容器是从镜像派生下来的，即容器是镜像初始状态的一次拷贝，后续容器可以创建快照和恢复快照。

## 启动容器

```
$ docker run -it ubuntu  [--name <container-name>]  /bin/bash  
```

**选项:**

-i: 代表交互，若不添加此选项，则输入命令无法响应
-t: 代表终端，若不添加此选项，因为没有阻塞，容器很快就退出了。 
-d: 后台运行，可以通过attach, exec 启动容器
--name: 给容器命名。
-p: 指定端口。


**attach**

退出终端时容器也会退出
```
$ docker attach <container-id>
```

**exec**

退出终端时容器不会退出。
```
$ docker exec -it <container-id>
```

## 查看所有的容器

```
$ docker ps -a
```
也可以使用下述命令
```
$ docker container ls -a
```

### 启动容器

```
$ docker start <container-id>
```

## 停止容器

```
$ docker stop <container-id>
```

## 后台运行

```
$ run i
```

## 导出容器

是保存当前的快照，然后导出镜像生成新的镜像，再从新的镜像里生成容器。
```
$ docker export <container-id>  >  ubuntu.tar
```
### 导入容器

将保存的快照导入镜像，生成新的镜像， `image-name`由自己指定生成的新镜像名称。 
```
$ cat <快照文件> | docker import - <image-name>
```

## 删除容器

```
$ docker rm <container-id>
```
-f: 强制删除正在运行的容器
-l: 移除容器的网络连接

删除所有终止的容器

```
$ docker container prune
```

## 查看容器的标准输出

```
$ docker logs <container-id>
```

选项:

-f: 让其自动滚动，类似`tail -f`

## docker访问主机设备

映射所有设备
> -v /dev:/dev


映射指定设备
> --device /dev/video0:/dev/video1

或者，假设您的USB设备在/dev/bus/usb中的主机上具有可用的驱动程序等，您可以使用特权模式和volumes选项将其安装在容器中.
> --privileged

## web容器

拉取镜像
```
$ docker pull training/webapp 
```

生成容器
```
$ docker run -d -P training/webapp python app.py
```

## [容器连接](./container-connect.md)

## 查看端口映射

```
$ docker port <container-id | container-name>
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