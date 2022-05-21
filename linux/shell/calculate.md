## 整形运算

### 自增

$[] 和 $(()) 都表示数学运算 

```shell
i=$[$i+1];

i=$(( $i + 1 ))
```

## 浮点数计算

带精度
```bash
echo "scale=2; ${score}/100" | bc
```

不带精度
```bash
echo "${score}/100.0" | bc
```