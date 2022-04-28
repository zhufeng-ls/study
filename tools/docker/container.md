# container

**中文:** 容器

容器是从镜像派生下来的，即容器是镜像初始状态的一次拷贝，后续容器可以创建快照和恢复快照。

## 启动容器
> 若镜像名不加版本，则默认匹配latest版本
```
$ docker run -it ubuntu  [--name <container-name>]  /bin/bash  
```

**选项:**

-i: 代表交互，若不添加此选项，则输入命令无法响应
-t: 代表终端，若不添加此选项，因为没有阻塞，容器很快就退出了。 
-d: 后台运行，可以通过attach, exec 启动容器
--name: 给容器命名。, 必须要放在镜像 id 后面


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

不接参数表示查看启动的容器
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

注意:
* -v 必须要放在镜像名前面

映射所有设备
> -v /dev:/dev


映射指定设备
> --device /dev/video0:/dev/video1

使docker 容器内的root权限为真正的root权限， 可以对host 和 设备进行操作，否则在docker内挂载设备会失败。\
假设您的USB设备在/dev/bus/usb中的主机上具有可用的驱动程序等，您可以使用特权模式和volumes选项将其安装在容器中。
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

## docker 显示图形界面
**参考网址**: \
https://blog.csdn.net/Frank_Abagnale/article/details/80243939#commentBox \
https://blog.51cto.com/u_15187242/2744702

1. 安装X11， 开放权限
```
$ sudo apt-get install x11-xserver-utils
$ xhost +
```
2. 启动 docker 容器时， 添加选项如下
```
 -v /tmp/.X11-unix:/tmp/.X11-unix            #共享本地unix端口
 -e DISPLAY=unix$DISPLAY                   #修改环境变量DISPLAY    把docker 的设置和主机一样
 -e GDK_SCALE                             #我觉得这两个是与显示效果相关的环境变量，没有细
   

```
**例如**：
```
$ docker run -d -p 6866:6800  -v /etc/localtime:/etc/localtime:ro   -v /tmp/.X11-unix:/tmp/.X11-unix   -e DISPLAY=unix$DISPLAY   -e GDK_SCALE   -e GDK_DPI_SCALE  --name scrapyd  willshory/scrapyd
```
3. 将 `xhost +` 命令添加到开机启动脚本中

## 添加域名
```
docker run 
--add-host=test.docker.com:192.168.1.9   
--add-host=test2.docker.com:192.168.1.10
--name se-chrome  se/chrome:3
```


## 挂载目录到docker中用来存放日志

`:`前面是宿主机目录，后面是容器目录
```
-v /var/local/:/opt/logs/
```

-v 后面加`:ro` 表示不可修改


## 当在容器的启动脚本里面操作时

比如我想要再docker 的启动脚本再开启一个守护脚本，不能只写一次，而是要在容器的启动脚本里面进行死循环，循环调用那个脚本，且将守护进程的功能放在容器的启动脚本中。