## 官方地址

[Docker Hub](https://hub.docker.com/)

## Docker Hub

### 注册

在 https://hub.docker.com 免费注册一个 Docker 账号。

## 登陆

```
$ docker login
```

## 注销

```
$ docker logout
```

## 推送镜像

打tag

```
$ docker tag ubuntu:18.04 username/ubuntu:18.04
```

推送

```
$ docker push username/ubuntu:18.04
```
