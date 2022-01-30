## go 的 mod 和 module 是什么？

### module

module 是官方推出的管理库的一种方式，通过 `GO111MODULE=ON` 启用。

库都是以 package 来组织的，每个 package 包含一个或多个文件，一个项目包含一个或多个 package, Go Module 就是一组统一打版和发布的 package 的集合，package 以子文件夹存储在 module 中，以 module.path + / + package.name 的形式来调用 package。

### go.mod

用来标记 module 和它的依赖库以及依赖库的版本, 放在 module 的主文件夹下。


## 使用使用Test进行测试模块

**步骤:**
- [ ] 在要测试的项目的子 package 目录创建测试文件，文件名为`xxx.test.go`, `xxx`为要测试的文件名
- [ ] 需要运行的方法命名`Test_Xxx(t *testing.T)`,Xxx为要测试的函数名称,当测试性能的时候参数命名为`b *testing.B`。
    
    **注意**：
    * 结构体的成员函数不能测试。
    * 若要跳过test文件中的某个方法，使用 `t.SkipShow()`
    * go test 不会按顺序执行多个 case, 若想要顺序执行，需要用到 subtests, 使用 t.Run() 来执行 subtests 可以起到控制顺序的目的。
    * 可以使用 TestMain 作为初始化 test。