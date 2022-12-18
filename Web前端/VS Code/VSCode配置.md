# VS Code配置

# 一、VS Code插件





# 二、windows单击右键【用VS Code打开文件夹】

1、新建文件vscode.reg，输入以下内容并保存，路径替换为`VS Code`安装路径；

```shell
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\*\shell\VSCode]
@="使用VS Code打开"
"Icon"="D:\\Programs\\VSCode-win32-x64-1.72.2\\Code.exe"


[HKEY_CLASSES_ROOT\*\shell\VSCode\command]
@="\"D:\\Programs\\VSCode-win32-x64-1.72.2\\Code.exe""
    
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\shell\VSCode]
@="使用VS Code打开"
"Icon"="D:\\Programs\\VSCode-win32-x64-1.72.2\\Code.exe"

[HKEY_CLASSES_ROOT\Directory\shell\VSCode\command]
@="\"D:\\Programs\VSCode-win32-x64-1.72.2\\Code.exe\" \"%V\""

Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\Background\shell\VSCode]
@="使用VS Code打开"
"Icon"="D:\\Programs\VSCode-win32-x64-1.72.2\\Code.exe"

[HKEY_CLASSES_ROOT\Directory\Background\shell\VSCode\command]
@="\"D:\\Programs\VSCode-win32-x64-1.72.2\\Code.exe\" \"%V\""
```



2、双击运行文件即可；