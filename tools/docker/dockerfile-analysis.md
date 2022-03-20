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

## 参数
参考: https://blog.csdn.net/u014042047/article/details/108768082 

指令	含义
FROM 镜像	指定新镜像所基于的镜像，第一条指令必须为FROM指令，每创建一个镜像就需要一条FROM指令
MAINTAINER 名字	说明新镜像的维护人信息
RUN命令	在所基于的镜像上执行命令，并提交到新的镜像中
CMD[“要运行的程序”，“参数1”，“参数2”]	指令启动容器时要运行的命令或脚本，Dockerfile只能有一条CMD指令，如果要指定多条则只能最后一条执行
EXPOSE 端口号	指定新镜像加载到Docker时要开启端口
ENV 环境变量 变量值	设置一个环境变量的值，会被后面的RUN使用
ADD 源文件/目录 目标文件/目录	将源文件复制到目标文件，源文件要与Dockerfile位于相同目录中，或者是一个URL
COPY 源文件/目录 目标文件/目录	将本地主机上得文件/目录复制到目标地点，源文件/目录要与Dockerfile在相同的目录中
VOLUME[“目录”]	在容器中创建一个挂载点
USER 用户名/UID	指定运行容器时的用户
WORKDIR路径	为后续的RUN、CMD、ENTRYPOINT指定工作目录
ONBUILD命令	指定所生成的镜像作为一个基础镜像时所要运行的命令
HEALTHCHECK	健康检查
————————————————
版权声明：本文为CSDN博主「琴酒3」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。


## 参数2
参考: 原文链接：https://blog.csdn.net/xiefeisd/article/details/88548411

Dockerfile的指令
类似上文的FROM、LABEL、RUN叫做指令，Dockerfile可用的指令如下。

序号	标签	功能	用法	说明
1	FROM	声明系统镜像	FROM <image> [AS <name>]	 
 	 	 	FROM <image>[:<tag>] [AS <name>]	 
 	 	 	FROM <image>[@<digest>] [AS <name>]	 
2	RUN	运行命令或文件	RUN <command>	创建镜像时运行
 	 	 	RUN ["executable", "param1", "param2"]	 
3	CMD	运行命令或文件	CMD ["executable","param1","param2"]	启动镜像时运行
 	 	 	CMD ["param1","param2"]	 
 	 	 	CMD command param1 param2	 
4	LABEL	设置说明性标签	LABEL <key>=<value> <key>=<value> <key>=<value>	 
5	MAINTAINER	设置作者	MAINTAINER <name>	弃用，用LABEL maintainer=替代
6	EXPOSE	暴露端口	EXPOSE <port> [<port>/<protocol>…]	仅起说明作用
7	ENV	设置环境变量	ENV <key> <value>	创建、启动镜像时均有效
 	 	 	ENV <key>=<value> …	 
8	ADD	添加文件	ADD [--chown=<user>:<group>] <src>... <dest>	 
 	 	 	ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]	 
9	COPY	复制文件	VOLUME ["/data"]	 
 	 	 	COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]	 
10	ENTRYPOINT	设置入口点	ENTRYPOINT ["executable", "param1", "param2"]	 
 	 	 	ENTRYPOINT command param1 param2	 
11	VOLUME	挂载卷	VOLUME ["/data"]	容器的文件操作针对挂载卷
12	USER	设置用户与组	USER <user>[:<group>]	 
 	 	 	USER <UID>[:<GID>]	 
13	WORKDIR	设置工作路径	WORKDIR /path/to/workdir	 
14	ARG	设置变量	ARG <name>[=<default value>]	仅创建镜像时有效
15	ONBUILD	设置触发指令	ONBUILD [INSTRUCTION]	用于构建另一个镜像时触发
16	STOPSIGNAL	设置停止信号	STOPSIGNAL signal	 
17	HEALTHVHECK	健康检查	HEALTHCHECK [OPTIONS] CMD command	检查命令执行状态
 	 	 	HEALTHCHECK NONE	 
18	SHELL	覆盖默认shell	SHELL ["executable", "parameters"]	 
