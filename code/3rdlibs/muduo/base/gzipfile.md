# gz

gz的压缩和解压，主要是对 libz.so库的使用。

一些比较简单的操作实现，就可以都放在头文件中。

## 实现
```
gzwrite
gzread
gztell
gzoffset
gzopen
gzbuffer
gzclose
```

## 扩展
gzopen 的参数, 类似于标准库中的 `open`。