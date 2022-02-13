## 文件无法删除问题

原因： 文件所属组不同，不能被非root 的其他用户删除

解决办法： 改变文件的所属组

```
$ chmod -hR  admin:admin dirname/
```