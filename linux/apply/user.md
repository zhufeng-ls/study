## 修改密码

### passwd

### chpasswd

批量修改密码。

**工作原理**

从系统的标准输入读入用户的名称和口令，然后更新密码。

```
$ echo user:pwd | chpasswd
```

**注意事项**
* 用户名必须是系统上存在的用户
* 普通用户没有使用这个指令的权限, 需要给chpasswd加`4755`权限。
* 如果输入的文件是按非加密方式传递的话，清对该文件进行适当的加码。
* 指令文件不能有空行

## 用户组中用户的添加和删除

[使用 gpasswd 添加和删除用户](../cmd/gpasswd.md)