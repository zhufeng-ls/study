# 文件操作

## 获取目录下最新的文件名

```
filename=$(ls -lt $docker_export_dir | grep $file_base | head -n1 |awk '{print $9}')
```