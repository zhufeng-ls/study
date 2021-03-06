## linux 环境命令

### 安装指定的库版本

  * 先查看可安装的版本

    ```
    sudo apt-cache madison  openssh-client
    ```

  * 在选定版本进行安装

    ```
    sudo apt-get install  openssh-client=1:6.6p1-2ubuntu1
    ```
### 查找库属于哪个安装包
 需要用到apt-file命令

  ```
  sudo apt-get install apt-file
  sudo apt-file update
  apt-file search libz.so.1
  ```

### 查看头文件属于哪个包
    ```
    apt-file search nss.h
    ```

### 搜索软件所在安装包
 ```
 dpkg -S /bin/ls
 ```

### 解决安装依赖

1. 若是因为软件安装包未安装
    ```
    $ sudo apt-get -f install
    ```
2. 若是因为库的版本问题，则需要安装`aptitude`
    ```
    $ sudo aptitude -f install
    ```

### 添加安装源自动安装

[添加安装源](./添加源.md#添加安装源)

### 删除安装包的配置文件

```
$ dpkg -l | grep ^rc | cut -d' ' -f3 | sudo xargs dpkg --purge
```