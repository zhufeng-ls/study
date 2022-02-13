## 安装

**安装包**: http://download.qt.io/archive/qt/

## 注意

**cannot find -lGL**

1. 可能是缺少库，安装库
   ```
   $ sudo apt install libgl1
   ```

2. 可能是没有.so的后缀库，为其创建个.so的软链接就可以。