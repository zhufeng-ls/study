# scp

远程拷贝命令

```
>>> 将本机文件远程拷贝到服务器
$ scp [option] /path/to/source/file user@serverip:/path/to/destination/directory

[option]选项:
-c: 这会在复制过程中压缩文件或目录。
-P: 若默认端口不是22，则用此选项指定端口
-r: 递归复制目录及其内容
-p: 保留文件的访问和修改时间

>>> 远程拷贝服务器文件到本机
$ scp [option]  user@serverip:/path/to/source/directory  /path/to/destination/file
```