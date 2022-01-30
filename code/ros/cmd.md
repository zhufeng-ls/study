# ROS命令

-----

### rospack

```
查找某个pkg的地址：
rospack find package_name
列出本地所有pkg:
rospack list

```

### roscd

```
跳转到某个pkg路径下
roscd package_name
```

### rosls

```
列出某个pkg 下的文件信息
rosls pakcage_name
```

### rosed

```
### 编辑pkg中的文件，相当于vi
roscd package_name file_name
```

### catkin_create_pkg

```
### 创建一个pkg
catkin_create_pkg  <pkg_name> [deps]
```

### rosdep

```
安装pkg所需要的依赖
rosdep install [pkg_name]
```

### rosnode

```
列出当前运行的node信息
rosnode list
显示某个node的详细信息
rosnode info [node_name]
结束某个node
rosnode kill [node_name]
```

