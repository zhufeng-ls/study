## 创建表

**unique:**
>指定某列唯一
```
create table {
    id int(10) NOT NULL unique
}
```

## 删除表

```
mysql> drop table 'tableName'
```


## 表内数据操作

### 清除表的内容

```
mysql> truncate 'tableName'
```

### 查看表的列信息

```
mysql> desc tableName;
```
