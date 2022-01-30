## cmake 编译 qt

1. 寻找qt包
```
find_package(Qt5 COMPONENTS Core Gui Widgets REQUIRED)
```

若找不到qt包，则在`/etc/profile`文件末尾添加
```shell
export PATH=/opt/Qt5.14.1/Tools/QtCreator/bin:$PATH
export QTDIR=/opt/Qt5.14.1/5.14.1/gcc_64 
export PATH=$QTDIR/bin:$PATH 
export LD_LIBRARY_PATH=$QTDIR/lib:$LD_LIBRARY_PATH
```

2. 设置 ui、qrc、qt相关的cpp文件自动编译选项
```
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)
```

也可以手动设置
```cmake
#调用预编译器moc，需要使用 QT5_WRAP_CPP宏
QT5_WRAP_CPP(SRCS_UI_M ${SRCS_UI})
#使用uic处理.ui文件
QT5_WRAP_UI(UI_FORMS_U ${UI_FORMS})
#使用rcc处理.qrc文件
QT5_ADD_RESOURCES(UI_RESOURCES_R ${UI_RESOURCES})

```

3. 添加链接动态库
```cmake
target_link_libraries(${PROJECT_NAME} ${Qt5Widgets_LIBRARIES} ${Qt5Core_LIBRARIES} ${Qt5Gui_LIBRARIES})
```
