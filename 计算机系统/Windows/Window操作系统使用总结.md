# WIndows操作系统使用总结.md

# 一、win11右键菜单回退到win10

**代码**

```shell
@echo off

:start

cls

echo,

echo 修改右键菜单模式

echo,

echo 1 穿越到Windows 10默认模式

echo,

echo 2 恢复为Windows 11默认模式

echo,

echo 0 什么也不做，退出

echo,

echo,

choice /c:120 /n /m:"请选择要进行的操作（1/2/0）："

if %errorlevel%==0 exit

if %errorlevel%==2 goto cmd2

if %errorlevel%==1 goto cmd1

exit

:cmd1

reg.exe add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve

taskkill /f /im explorer.exe

start explorer.exe

exit

:cmd2

reg.exe delete "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f

taskkill /f /im explorer.exe

start explorer.exe

exit
```

- 新建一个文档，将以上代码粘贴进去。

- 选择另存为

![](D:\Nanum-Note\Operation System\Windows\img\win11菜单回退-01.png)

- 保存后右键选择`以管理员身份运行`；

- 根据提示操作即可；

![](D:\Nanum-Note\Operation System\Windows\img\win11菜单回退-02.png)



# 二、删除服务

```shell
sc delete "服务名"
```

# 三、删除端口占用进程

1、先找到占用端口的进程id（pid）

```shell
netstat -ano | findstr [端口号]
```

2、杀死进程

```shell
taskkill /f /pid [pid]
```

# 四、DOS命令

1. dir Directory查看当前目录下所有文件夹
2. cls  CleanScreen清除屏幕
3. exit 退出
4. d：切换盘符
5. cd+空格+路径  ChangeDirectory，切换路径
6. cd .. 切换到上一级目录
7. tab键可以自动补全
8. 上键和下键可以切换历史命令记录