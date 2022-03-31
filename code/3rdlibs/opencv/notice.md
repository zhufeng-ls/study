# 错误和应用

## 应用

**图片叠加时的技巧**

可以载入灰度图像最掩码，这样的图片可以做到完全覆盖叠加。


## 错误

**error: (-2:Unspecified error) Can't initialize GTK backend in function 'cvInitSystem'**

改环境变量，即`export DISPLAY=:0.0`，等价于 `export DISPLAY=localhost:0.0`
