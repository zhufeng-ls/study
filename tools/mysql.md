## mysql 修改密码

**步骤:**

指定数据库:
```
mysql> use mysql;
```

重新加载权限表:
```
mysql> flush privileges;
```

设置密码:
```
mysql> alter use 'root'@'localhost' identified with mysql_native_password by "密码";
```

重新加载权限表
```
mysql> flush privileges;
```
