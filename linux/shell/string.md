# 字符串操作

## 拆分为数组

IFS为设置的分隔符号， 下例中表明 `,` 和 ` `（空格） 作为分隔符号。

```
IFS=', ' read -r -a array <<< "$string"
```

## 拆分截取某一段

```bash
cut -d':' -f2 <filename>
```

以':' 为分隔符合进行产品，截取第二段(从下标0开始)

## 排序

```shell
sort -t ' ' -k 2 -r
```

* -t: 设置分隔符号
* -k: 排序第二列
* -r: 降序
* -u: 去重


## 打印输出的某一列

```shell
ls -lh | awk '{print $2}'
```

## 替换掉某些输出

```shell
echo "123.tar" | sed 's/.tar//'
```
