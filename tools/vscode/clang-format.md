# vscode 自动格式化

参考网址:

https://blog.csdn.net/weixin_43728093/article/details/119953734

https://www.cnblogs.com/oloroso/p/14699855.html

## 首先安装 clang-format

```bash
sudo apt-get update && sudo apt-get install clang-format-9
```

创建软链接

```bash
sudo ln -s /usr/bin/clang-format-9 /usr/bin/clang-format
```

查看是否安装成功
```bash
clang-format --version
```

若打印如下，则安装成功
```
clang-format version 9.0.0-2~ubuntu18.04.2 (tags/RELEASE_900/final)
```

## 在 vscode 项目中使用 clang-format

### 打开 vscode 的 json 设置文件
1. 输入快捷键`ctrl + .`, 在上方的输入框中输入 settings, 找到 settings.json 并进入
2. 输入以下内容
   ```json
   {
       // 保存就自动格式化
       "editor.formatOnSave": true,
       // 设置 clang-format 的二进制路径
       "clang-format.executable": "/usr/bin/clang-format",
   }
   ```
3. 要记得屏蔽上述文件中的 `"C_Cpp.clang_format_style": "Google"`, 否则clang-format 不会生效。


### 拷贝 clang-fomat 文件在当前工作根目录中
```bash
clang-format -style=llvm -dump-config > .clang-format
```

现在就可以生效了，可以试着在 cpp 或 c 文件中 `ctrl +s` 保存，来格式化代码。
