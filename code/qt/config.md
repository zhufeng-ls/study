## 环境变量配置

> export PATH=/opt/Qt5.14.1/Tools/QtCreator/bin:\$PATH 

> export QTDIR=/opt/Qt5.14.1/5.14.1/gcc_64

> export PATH=\$QTDIR/bin:$PATH 

> export LD_LIBRARY_PATH=\$QTDIR/lib:\$LD_LIBRARY_PATH 

**注意**: 如果使用 apt 安装了qt包，$PATH需放在后面，确保找库时先从我们手动安装的路径去寻找。
