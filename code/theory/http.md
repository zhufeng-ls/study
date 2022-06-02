## http数据类型

### form
form数据是表单数据类型，每段数据都是分开的，不同段的数据可以指定不同的数据格式。

## http 头

长短链接
* `Connection: close`: 表示传输完立刻断掉，即使用短链接
* `Connection: Keep-Alive`: 表示使用长链接
* `Content-Length: 201 `: 表示传输内容的长度，即 body， 不包含头
  

## 依赖

muduo::net::Buffer