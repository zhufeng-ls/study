## 生成core duump 文件
**程序配置**:\
1. 加上-g选项
2. 选用-debug调试模式

**系统配置**:\
参考网址: \
https://blog.csdn.net/Jqivin/article/details/121908435


### 步骤

1. 判断是否打开了coredump
    ```
    $ ulimit -c 
    ```
    输出 `0`则代表未打开。\
    临时打开: `ulimit -c unlimited`。 \
    永久打开: \
    打开文件/etc/sysctl.conf
    ```   
    kernel.core_pattern = /var/crash/core_%e_%p                                                                                                                                                                                     kernel.core_uses_pid = 1                                                                          
    ```


2. 修改/etc/default/apport文件，enabled 设置为0
   ```
   enabled=0
   ```


3. 重启
4. 触发段错误， /var/crash/目录下会生成对应的文件。


### 注意

1. 当 /var目录权限有限制时，只有通过 `sudo` 运行的程序才能够保存 `coredump` 文件。
2. 在docker 里面生成core文件时，会以主机的配置为主(存疑？)
