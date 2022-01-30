

* 环境变量

  * set

    显示当前shell的变量

  * env

    显示用户的变量

  * export

    显示和设置当前用户的变量

    ```
    增加环境变量
    export TEST = 'test'
    ```
    显示当前所有的环境变量
    ```
    $ export
    ```
    

  * unset

* 更新环境变量

```
$ source /etc/profile
```

**注意**: 更新的环境变量只在当前终端有效, 重启后才能在所有终端生效
  