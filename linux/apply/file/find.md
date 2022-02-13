## 查找文件

### find 

> 从硬盘中找文件，非常耗时。

可选项有：

* -name
* -type
    * f 一般文件
    * d 目录
    * c 字符设备文件
    * b 块设备文件
    * l link文件
    * s socket文件
* -size 文件大小 单位是 b(blocks) c（字符数） k（kilo bytes） M G


### locate

> 从保存文档和目录名称的数据库中查询文档或目录。

```
$ locate filename
```

可选项:

* -i --ignore-case 忽视大小写
* -c --count 输出数量

存储文件的数据库路径为 `/var/lib/mlocate/mlocate.db`, 每天都会自动更新一次，也可以手动更新。

```
$ updatedb
```
