# 项目目录结构

参考网址: \
https://cnblogs.com/kuliuheng/p/5729559.html \
https://github.com/hattonl/cpp-project-structure

project --+---build---+---debug \
　　　　　|　　　　  |---release \
　　　　　|---dist \
　　　　　|---doc \
　　　　　|---include---+---module1 \
　　　　　|　　　　　 　|---module2 \
　　　　　|---lib \ 
　　　　　|---src---+---module1 \
　　　　　|        |---module2 \
　　　　　|---res \
　　　　　|---samples---+---sample1 \
　　　　　|　　　　  　　|---sample2 \
　　　　　|---test \
　　　　　|---examples \
　　　　　|---tools \
　　　　　|---scripts \
　　　　　|---copyleft \
　　　　　|---CMakeLists.txt \
　　　　　|---README \
　　　　　|--- ... 

下面分别介绍一下各目录和文件的用途

build/:项目编译目录，各种编译的临时文件和最终的目标文件皆存于此，分为debug/和release/子目录

dist/:分发目录，最终发布的可执行程序和各种运行支持文件存放在此目录，打包此目录即可完成项目分发

doc/:保存项目各种文档

include/:公共头文件目录，可以按模块划分组织目录来保存模块相关头文件

lib/:外部依赖库目录

res/:资源目录

samples/:样例程序目录

examples: 存放测试小demo。

test/: 存在测试用例

tools/:项目支撑工具目录

scripts/ : 包含一些脚本文件，如使用Jenkins进行自动化构建时所需要的脚本文件，以及一些用于预处理的脚本文件。

copyleft:版权声明文件，当然也可以叫做copyright :-)

CMakeLists.txt:项目构建配置文件，当然也有可能是其他类型的构建配置文件,比如bjam

README:项目的总体说明文件