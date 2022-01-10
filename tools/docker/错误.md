## 当前用户启动错误

安装完docker后，运行命令出现

>”Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.26/images/json: dial unix /var/run/docker.sock: connect: permission denied“

### 原因

docker进程使用Unix Socket而不是tcp接口， 而默认情况下， Unix Socker 属于root用户， 需要root权限才能访问。

### 解决办法

方法一:

使用`sudo`获取管理员权限， 运行docker组。

方法二:

docker守护进程启动的时候，会默认赋予名字为docker的用户组读写Unix socket的权限， 因此需要将当前用户添加进入docker用户组中，那么组内的用户就有权限访问Unix socket了。

```
$ sudo groupadd docker #添加docker用户组
$ sudo gpasswd -a $USER docker #将登陆用户添加到docker用户组中
$ newgrp docker #更新用户组
$ docker ps #测试docker命令
```

## 登陆docker错误

普通用户登陆docker，报文件权限错误。

> WARNING: Error loading config file: /home/jim/.docker/config.json - stat /home/jim/.docker/config.json: permission denied
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.

### 原因

当前用户没有打开docker配置文件的权限。

### 解决办法

将当前用户添加到docker组中，并更改文件所属组。
```
$ sudo gpasswd -a ${USER} docker
$ sudo systemctl restart docker
$ sudo chown "${USER}":"${USER}" /home/"${USER}"/.docker -P
$ sudo chmod g+rwx "/home/${USER}/.docker" -R
```