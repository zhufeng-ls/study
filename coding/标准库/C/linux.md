## GNUC 

表示 gcc 编译器

**GNUC** 、**GNUC_MINOR** 、**GNUC_PATCHLEVEL**分别代表gcc的主版本号，次版本号，修正版本号

在 C/C++ 中使用。

```
#define GCC_VERSION (__GNUC__ * 10000 \
                           + __GNUC_MINOR__ * 100 \
                           + __GNUC_PATCHLEVEL__)
    ...
    /* Test for GCC > 3.2.0 */
    #if GCC_VERSION > 30200
```


