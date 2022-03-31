## 镜像

当运行容器时，若使用的镜像在本地不存在， docker就会自动从docker镜像仓库中下载， 默认是从Docker Hub公共镜像源中下载。

**常用的镜像有:**
ubuntu: 
training/webapp: web应用镜像 

### 列出镜像列表

有两种方法。

```
$ docker images
```

```
$ docker image ls -a
```

**字段说明**

* REPOSITORY: 镜像的仓库源
* TAG: 镜像的标签
* IMAGE ID: 镜像ID
* CREATED: 镜像创建时间
* SIZE: 镜像大小

同一个仓库源可以有多个 TAG。
```
$ docker run -t -i ubuntu:14.04 /bin/bash
```

若是不指定标签，将默认使用`ubuntu:latest`镜像。

### 获取新的镜像

```
$ docker pull <image-name>:<标签>
```

### 查找镜像

```
$ docker search httpd
```

**字段说明**

* NAME: 镜像仓库源的名称
* DESCRIPTION: 镜像的描述
* OFFICIAL: 是否官方发布
* stars: 表示点赞、喜欢的意思。
* AUTOMATED: 自动构建。

### 删除镜像

```
$ docker rmi hello-world
```

### 更新镜像

创建容器后，在运行的容器内使用`apt-get update`命令来更新，更新后，使用exit退出容器。然后使用`docker commit`来提交容器副本。

```
$ docker commit -m="has update" -a="runood" <container-id> runoob/ubuntu:v2
``` 

**参数说明**

* **-m**: 提交的参数信息
* **-a**: 指定镜像作者
* **runoob**: 要创建的目标镜像名

### 构建镜像

需要创建一个Dockerfile文件，告诉docker来如何构建我们的镜像。
```
FROM centos:6.7
MAINTAINER Fisher "fisher@sudops.com"

# RUN /bin/echo 'root:123456'|chpasswd
RUN useradd runoob
RUN /bin/echo 'runoob:123456'|chpasswd
RUN /bin/echo -e "LANG=\"en_US.UTF-8\"">/etc/default/local
EXPOSE 22
EXPOSS 80
CMD /usr/sbin/sshd -D
```

然后使用 `docker build`创建镜像。
```
$ docker build -t "runnoob/myImage:1.0" dir/
```

**参数说明**
* **-t**: 指定要创建的镜像名称。

## 为镜像打标签
> 若不加版本，则默认为latest
```
$ docker tag <image-id> runoob/myImage:tagName
```
## 推送镜像

打tag

推送

```
$ docker push username/ubuntu:18.04
```

## 查看镜像详细信息

```
$ docker inspect <镜像id> 
```
