# 使用 dockerfile 定制镜像

定制一个nginx镜像，构建好的镜像内会有一个`/usr/share/nginx/html/index/html`

## 在一个空目录下，新建一个名为dockerfile文件，并在文件中添加以下内容

> FROM nginx
> RUN echo '这是一个本地构建的nginx镜像' > /usr/share/nginx/html/index.html


**From 和 Run 指令的作用**

**From**: 定制的镜像都是基于From的镜像，这里的nginx就是定制需要的基础镜像。后续的操作都是基于nginx。
**RUN**: 用于执行后面跟着的命令行命令。有以下两格式：

shell格式:
```
RUN <shell>
```  

exec格式:
```
RUN ["可执行文件"， "参数1"， "参数2"]
```


注意: dockerfile的指令每执行一次都会在docker上新建一层，所以过多无意义的层， 会造成镜像膨胀过大，因此指令最好用 && 连接起来

例如：
> FROM centos
> RUN yum -y install wget
> RUN wget -O redis.tar.gz "http://download.redis.io/..."
> RUN tar -xf redis.tar.gz

以上执行会创建3层镜像。可简化下以下格式:
> FROM centos
> RUN yum -y install wget
> RUN wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz"
> RUN tar -xvf redis.tar.gz

## 开始构建镜像

```
$ docker build -t nginx:v3 ./
```

