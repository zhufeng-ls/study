# 语法

## 全局变量

若在函数内部修改全局变量的值，必须要先声明
```python
a = 0

def fun():
    global a
    a = 7
```