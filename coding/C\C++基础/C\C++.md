### 全局变量的初始化

c 语言中全局变量的初始化是在编译时完成的，

c++ 对普通 全局变量的处理和 c++ 一样， 对复杂全局变量（如自定义类）的处理是>main 函数之前处理的，即运行时。

但是也和编译器有关。

### main 参数解析

```
argc: 命令行参数个数，至少一个，因为argv[0]为执行程序名。
argv: 命令行参数，argv[0]为可执行程序名。例如，为a.out,其他参数正常排列在后面
```

### c++ 中NULL 和 nullptr的区别

```
void  func(void *t){
    cout << "func1" <<endl;
}
void  func(int i){
    cout << "func2" << endl;
}
int main (){
    func(NULL);
    func(nullptr);
    return  0;
}
  ```

  // 结果输出为：func2，
               func1。
