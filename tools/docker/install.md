到目前为止，在ubuntu20.04上使用wechat最简单的方式不是wine，而是用> > docker。

今天就传授大家一个一定可以使用的docker安装的wine版本。

首先，安装一下docker：
```
$ sudo apt install docker.io
$ sudo systemctl enable --now docker
```

```
$ sudo service docker start
$ docker pull bestwu/wechat
```

## 记录下这个数值

要要用到 `AUDIO_GID` 中

```
$ getent group audio | cut -d: -f3
```

接下来要做的事情就是创建一个yml配置文件用来每次启动wechat：

```
$ gedit docker-wechat.yaml
```

```
version: '2'
services:
  wechat:
    image: bestwu/wechat
    container_name: wechat
    devices:
      - /dev/snd
    volumes:
      - /tmp/.X11-unix:/tmp/.X11-unix
      - $HOME/WeChatFiles:/WeChatFiles
    environment:
      - DISPLAY=unix$DISPLAY
      - QT_IM_MODULE=fcitx
      - XMODIFIERS=@im=fcitx
      - GTK_IM_MODULE=fcitx
      - AUDIO_GID=29 # 可选 默认63（fedora） 主机audio gid 解决声音设备访问权限问题
      - GID=1000 # 可选 默认1000 主机当前用户 gid 解决挂载目录访问权限问题
      - UID=1000 # 可选 默认1000 主机当前用户 uid 解决挂载目录访问权限问题
```