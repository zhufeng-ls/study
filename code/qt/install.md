## 安装

**安装包**: http://download.qt.io/archive/qt/

## 注意

**cannot find -lGL**

1. 可能是缺少库，安装库
   ```
   $ sudo apt install libgl1
   ```

2. 可能是没有.so的后缀库，为其创建个.so的软链接就可以。


## docker 无界面安装
1. 更新apt
    ```
    $ apt-get update
    ```
2. 添加ppa
    ```
    $ apt-get install software-properties-common
    ```
3. 添加qt ppa \
    qt版本的PPA见 https://launchpad.net/~beineri
   ```
   $ add-apt-repository ppa:beineri/opt-qt-5.12.2-bionic
   $ apt-get update
   ```
4. 安装 qt \
   按需求安装包，一般选择 minimal。
   * qt512-meta-minimal： Minimal core pacakges
   * qt512-meta-full：  The full stack (without *-no-gpl packages)
   * qt512-meta-dbg-full： Dbg packages (only for x64, not for all modules)

    安装qt：
    ```
    $ apt install qt512-meta-minimal
    ```

5. 更新环境变量
    ```
    $ Source /opt/qt512/bin/qt512-env.sh
    ```
