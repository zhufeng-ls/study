# 文件功能

## 文件追加内容

文件打开
```c
::fopen(filename.c_str(), "ae");
```

* e: 表示 当 fork 线程时，会自动关闭已经打开了的文件描述符
* a: 表示添加在文件末尾


设置文件的读写缓冲区
```c
::setbuffer(fp_, buffer_, sizeof buffer_);
```


开始写文件
```
::fwrite_unlocked(logline, 1, len, fp_)
```

fwrite是线程安全的，fwrite_unlocked是线程不安全的，但是效率高。


或者文件错误
```c
ferror(fp_);
```

fp_ 是 fopen 返回的描述符

## 读取小文件

**readToString**

模板类， 传入的类型可以为 vector 或 std::string 或 我们自己实现的String


**readToBuffer**

读到类自己的缓冲区内，然后从缓冲器里面去取。

并实例化一个模板类
```c++
template int FileUtil::ReadSmallFile::readToString(
    int maxSize,
    string* content,
    int64_t*, int64_t*, int64_t*);
```

现在就可以直接对 string 类型使用 readToString 



