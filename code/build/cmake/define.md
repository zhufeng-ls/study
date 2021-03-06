* cmake 宏

  > CMAKE_C_COMPILER：指定C编译器
  > CMAKE_CXX_COMPILER：指定C++编译器
  >
  > CMAKE_SIZEOF_VOID_P:  为4 代表32位平台，为8 代表64为平台
  >
  > CMAKE_POSITION_INDEPENDENT_CODE: 为 ON 代表产生与位置无关的代码，OFF关闭
  >
  > PROJECT_SOURCE_DIR ：当前项目的路径
  >
  > PROJECT_BINARY_DIR ：项目编译后存放的路径
  >
  > CMAKE_CURRENT_BINARY_DIR ：与PROJECT_BINARY_DIR基本相同
  >
  > CMAKE_CURRENT_SOURCE_DIR ：与PROJECT_SOURCE_DIR基本相同
  >
  > CMAKE_MODULE_PATH: 用于指定要由CMake加载的CMake模块的搜索路径[`include()`](https://cmake.org/cmake/help/v3.5/command/include.html#command:include) 或者 [`find_package()`](https://cmake.org/cmake/help/v3.5/command/find_package.html#command:find_package)命令，然后检查CMake随附的默认模块。默认情况下，它是空的，打算由项目设置。
  >
  > CMAKE_EXPORT_COMPILE_COMMANDS: 在生成期间启用/禁用编译命令的输出。如果启用，将生成一个compile_commands.json文件，其中包含m中项目所有翻译单元的确切编译器调用。
  >
  > CMAKE_SOURCE_DIR: 最顶层的CMakeLists.txt所在的目录 

  > EXECUTABLE_OUTPUT_PATH: 设置程序的输出路径
  > LIBRARY_OUTPUT_PATH： 设置库的输出路径

  > CMAKE_CXX_STANDARD: 指定 c++ 的版本