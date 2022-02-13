## 拆分文件，为了推送git

```
$ split "源文件名" -C 50M -d "拆分文件名" 
```

## 合并拆分的文件

```
$ cat test.exe* > test.exe
```

## 文件无法删除问题

原因： 文件所属组不同，不能被非root 的其他用户删除

解决办法： 改变文件的所属组

```
$ chmod -hR  admin:admin dirname/
```

## 输出文件的统计信息

```
$ $ wc testfile           # testfile文件的统计信息  
3 92 598 testfile       # testfile文件的行数为3、单词数92、字节数598 
```