# VSCode

## 添加头文件路径

1. F1 打开命令行。
2. 选择C/C++:Edit Configuration(JSON)， 将生成 c_cpp_properties.json 配置文件。
3. 编辑配置文件的 includePath选项。

## 更改 vscode 代码风格
1. 下载插件，分别为了阅读`cpp`代码和格式化

    ![plugins](./images/vscode_1.png)
2. 备份原有的格式文件(若是命令执行失败说文件没有生成，并不影响后面的操作)

    xxx为用户名
    ```
    $ cp /home/xxx/.config/Code/User/settings.json /home/xxx/.config/Code/User/settings.json.bak
    ```
3. 将下述内容写入`/home/xxx/.config/Code/User/settings.json`文件中
    ```json
    {
        "editor.fontSize": 12,
        "terminal.integrated.fontSize": 12,
        // 调整字体
        "editor.fontFamily": "'monospace', monospace, 'Droid Sans Fallback'",
        // 调整tab空格大小
        "editor.tabSize": 4,
        // 使 tab 的更改生效
        "editor.detectIndentation": false,
        "editor.renderControlCharacters": true,
        "editor.formatOnSave": true,
        "editor.wordWrap": "on",
        // "emmet.triggerExpansionOnTab": true,
        "[go]": {
            "editor.insertSpaces": false,
            "editor.formatOnSave": true,
            "editor.codeActionsOnSave": {
                "source.organizeImports": false
            }
        },
        // 调整 cpp 格式
        // "C_Cpp.clang_format_style": "Google",
        "C_Cpp.clang_format_fallbackStyle": "Google",
        "clang-format.executable": "/usr/bin/clang-format",
        "workbench.colorTheme": "Ayu Mirage Bordered",
        "workbench.iconTheme": "vscode-icons",
        "explorer.confirmDelete": false,
        "[cpp]": {
            "editor.defaultFormatter": "ms-vscode.cpptools"
        },
        // 默认以 CPP 格式打开文件
        "editor.defaultFormatter": "ms-vscode.cpptools",
        "clang.executable": "/usr/bin/clang-format",
        "cmake.configureOnOpen": true,
    }
    ```
4. 重启vscode

## 让隐藏字符现身

添加配置

>"editor.renderControlCharacters": true

## vscode 无法输入中文

原因: 缺少插件。

解决： 安装 snap。
```
$ sudo apt install snap
```

## vscode 无法选中整行

输入法出现了问题，切换输入法可以解决

5. 调试程序

## 自动换行
* ctrl + shift + P , 输入 settings, 打开 open user settings
* 搜索 wrap, 找到 editor:word wrap
* 将设置改为 on 选项