### cmake判断平台
  
  ```cmake
  if(WIN32 AND NOT MINGW)
        message("WIN32 编译设置")
     elseif(WIN32 AND MINGW)
       message("MINGW 编译设置")
     elseif(UNIX AND NOT ANDROID)
       message("UNIX 编译设置")
     elseif(ANDROID)
      message("ANDROID 编译设置")
     endif()
     
    //： CMAKE_SIZEOF_VOID_P # 为4 代表32位平台，为8 代表64为平台
  ```

### cmake 链接 opencv
```
find_package(OpenCV REQUIRED)
  
message(STATUS "OpenCV library status:")
message(STATUS "    version: ${OpenCV_VERSION}")
message(STATUS "    libraries: ${OpenCV_LIBS}")
message(STATUS "    include path: ${OpenCV_INCLUDE_DIRS}")

add_executable(example main.cpp)
target_link_libraries(example ${OpenCV_LIBS})
```

### 数组添加到变量中
```
set (lists
     ${list1}
     ${list2}
)
```