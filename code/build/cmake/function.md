  | cmake_minimum_required（）                            | VERSION后面加版本号                                          | 表示所需最小版本                                             |
  | ----------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | project()                                             | LANGUAGE 加  C  CXX                                          | 表示需要的编程语言，LANGUAGE可省略                           |
  | set(ASC  1)                                           | set里面分别为变量和值                                        | 给变量赋值，包括宏变量                                       |
  | IF(A  EQUAL  8)    ELSE()    ENDIF()                  | 判断条件                                                     | IF里面为条件，后面可接set语句                                |
  | set(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS}  -std=c++11" | 是c++的编译选项                                              | 代表加入c++11支持，类似的语句有add_compile_options           |
  | add_compile_options(-fPIC)                            | 增加编译选项                                                 | -fPIC代表产生与位置无关的代码，及产生的都是相对位置，代表可以在任何地方执行，动态库必备。 |
  | add_definitions(-DDEBUG)                              | 增加编译选项，也可以增加预处理定义                           | 例如，add_definitions(-DDEBUG)，和add_compile_options基本相同 |
  | file(glob Sources *.cpp)                              | 将所有的.cpp存到Sources中                                    |                                                              |
  | add_executable(Cars $(Sources) $(Includes))           | 用所有.cpp，.h文件编译成名为Cars的可执行文件                 |                                                              |
  | include_directories(${PROJECT_SOURCE_NAME}/include)   | 指定包含头文件的目录                                         |                                                              |
  | link_directories("")                                  | 添加链接库的路径，一次可添加多个                             |                                                              |
  | link_libraries("")                                    | 添加要链接的库文件名称，一次可添加多个                       |                                                              |
  | add_definitions("-DONNX_NAMESPACE=${ONNX_NAMESPACE}") | 在源文件的编译中添加-D define标志。                          |                                                              |
  | add_subdirectory(third_party/onnx ${BINARY_DIR})   | 新添加一个目录位置，编译这个目录中所有的内容，一般这个目录中也会包含CMakeLists文件, 生成的目录由自己指定 |                                                              |
  | find_package(OpenCV REQUIRED)                         | 这个命令是cmake中经常使用的命令，如果我们想在cmake中使用一些其他的大型开源项目(编译好的)，例如OpenCV，在我们将OpenCV编译好之后，如果我们想使用它，我们就可以在cmake中添加。 |                                                              |
  | list(append GPU_ARCHS  51 61 75)                      | 往GPU_ARCHS里面添加51，61，75变量                            |                                                              |
  | config.cmake                                          | 如果需要我们的CMakeLists有一定的自由配置，比如，我们需要开启CUDA的支持，或者关闭某个功能。如果功能项比较多的话，每次增加功能或者修改，直接在CMakeLists中写一堆代码命令会很麻烦。在这种情况下的话，最好是另外创建一个名为`config.cmake`的文件，这个文件中填写了我们的配置信息(举个例子)： |                                                              |
  | include(install_package)                              | 引入install_package.cmake模块,设置CMAKE_MODULE_PATH可以改变他的当前目录 |                                                              |
  | if(COMMAND cmake_policy)                              | 检查cmake_policy函数或宏是否被定义                           |                                                              |
  | if(DEFINED address)                                   | 检查是否定义address这个变量                                  |                                                              |
  | option(BUILD_TEST  "Build  test"  ON)                 | 设置cmake编译时链接命令选项，默认为ON                        |                                                              |
  | aux_source_directory(`<dir> <variable>`)              | 查找该目录下的所有源文件，并将值赋予`variable`


## file(GLOB variable RELATIVE \<path\>  ./*.cc)

表示从 path 查找当前目录下所有的.cc 后缀的文件， 存放在 RELATIVE中， 返回的路径是对path的相对路径。

## option(BUILD_TEST  "Build  test"  OFF) 

cmake 外部使用通过 `cmake .. -DBUILD_TEST=ON`来启用或关闭。

判断启用或关闭则通过:
```cmake
if(BUILD_TEST)
    xxx
endif()
```

## 注意

### add_library

默认的修饰方式是`static`, 若想生成动态库，则需要添加 `-shared` 或者 `SHARED`, 且必须要在
当前位置加，通过add_definitions() 或 add_compile_options添加则没有效果.


### macro 定义函数

若把传入的参数当作列表，在for循环时往内部添加值， 则循环结束时其列表仍然为空。

解决方法:
使用`set(listname "")` 创建一个临时列表，往listname填充值， 最后再用 set 将其值赋值给传入的接收参数中。



