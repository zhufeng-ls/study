## 设置代理
```
$ git config --global https.proxy http://192.168.0.101:808
```
注： 设置后的代理若为--global, 则保存在 ~/.gitconfig下， 若为 local， 则保存在 .git/config 下.

## git 版本升级
1. 添加源
    ```
    $ sudo add-apt-repository ppa:git-core/ppa
    ```
2. 更新安装列表
    ```
    $ sudo apt-get update
    ``` 
3. 安装
    ```
    $ sudo apt install git
    ```