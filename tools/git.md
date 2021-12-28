### 设置代理
```
$ git config --global https.proxy http://192.168.0.101:808
```
注： 设置后的代理若为--global, 则保存在 ~/.gitconfig下， 若为 local， 则保存在 .git/config 下.

### 问题

**Write access to repository not granted**

问题: 生成的token 权限太低，无法访问。

解决: 重新生成更高权限的token。
~                             
