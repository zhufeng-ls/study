## stash 

恢复stash 但不删掉记录。

```
$ git stash apply <stash-id>
```

恢复stash 并清除掉这条记录

```
$ git stash pop <stash-id>
```

删除一个stash 存储

```
$ git stash drop <stash-id>
```

删除所有的 stash 存储

```
$ git stash clear
```