### cmake修饰词

1. REQUIRED: 如果找不到软件包，该 选项将停止处理并显示一条错误消息。

2. FATAL_ERROR: 错误会导致cmake 直接退出

   例: cmake 版本小于3.2 直接报错退出

   ```
   cmake_minimum_required(VERSION 3.2 FATAL_ERROR)
   ```

   

3. SEND_ERROR: 错误会继续运行，但是最后结果显示的是错误(因为很多错误类似 include_directories()的错误并不影响编译)。

4. DESTINATION: 指定了一个文件会安装到磁盘的哪个路径下。若果给出的是全路径（以反斜杠或者驱动器名开头），它会被直接使用。如果给出的是相对路径，它会被解释为相对于CMAKE_INSTALL_PREFIX的值的相对路径。

5. FOLLOW_SYMLINK_CHAIN: 是否递归解析当前符号链接，直到找到对应的真实文件。
 
    应用场景: 使用`file(COPY ...)` 方法时可以选择拷贝的文件性质。

6. RELATIVE(英文释义: 相对而言的、关系)： 如果指定了此属性， 则返回的结果将是指定路径的相对路径(可以用于 `file` 查找文件)

7. APPEND: 往列表中添加值。

     例如: 
     ```
     list(APPEND dirlist {dir})
     ```