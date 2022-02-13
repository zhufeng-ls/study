# 添加安装源

- [ ] 查看发行版本
    ```
    $   lsb_release -a
    ```
- [ ] 根据发行版本的不同更换不同的不同的字段。
  
    我的发行版本是bionic, 对应着ubuntu18.04 的 LTS版本。

    编辑`/etc/apt/sources.list` 或 `/etc/apt/sources.list.d/目录下的文件`。

    * [sources.list](../../config/etc/apt/sources.list)
    * [sources.list.d](../../config/etc/apt/sources.list.d/README.md)

- [ ] 更新ubuntu源并下载源包
    ```
    $ sudo apt-get update
    $ sudo apt-get upgrade
    $ sudo apt-get install build-essential
    ```