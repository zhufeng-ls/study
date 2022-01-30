## mod

mod 是go语言的包管理方式，通过下面命令开启
```
// 改变包的下载源，科学上网可省略这一步
$ go env -w GOPROXY=https://goproxy.cn,direct
// 启用module
$ go env -w GO111MODULE=on
```

通过下面命令生成`go.mod`
```
$ go mod init 
$ go tidy

```

若开启了go modules后，编译时编译器会从工作目录（当前目录）开始并逐级向上查找是否具有 go.mod 文件。

* 如果有， go.mod文件中声明的 `module` 名称就视作 `go.mod` 所在的路径，然后以指定的 main 包作为依赖入口, 所有以 go.mod 中声明的 module 名称开头的导入路径都以 go.mod 所在的路径为相对路径进行包的查找导入。所有需要导入的路径中如果在 go.mod 中指定了版本，则从 $GOPATH/pkg/mod/ 下取得相应版本进行导入，如果没有被指定则从 $GOPATH/src/ 或 $GOROOT/src/ 中进行查找导入。
* 如果没有，所有依赖均从 `$GOPATH/src/` 或 `$GOROOT/src/` 中进行查找导入。

## 注意

当使用mod时， 将不会从 $GOPATH 下查询包

## 英文单词

modules: 模块
