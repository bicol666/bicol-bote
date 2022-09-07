# IntelliJ IDEA使用总结.md


# 问题总结

### dependencies输入框异常问题

如图，当我在输入框输入`servlet`时，输入框显示异常，最终显示输入内容为`slet`。

![image-20220511115516926](D:\Nanum-Note\Java\IntelliJ IDEA\IntelliJ IDEA使用总结-img\image-20220511115516926.png)

**解决办法**

暂无



# 常用快捷键

### 重构/重命名

```shell
shift + F6
```



### 重构/移动类（移包）

```shell
F6
```



### 切换注释文档渲染视图

```shell
Ctrl + Alt + Q
```



### 导包优化

```shell
Ctrl + Alt + o
```



### 运行

- 以调试模式运行

```shell
Shift + f9
```

- 直接运行

```shell
Shift + f10
```



# 常用配置

### 自动导包

![image-20220720110021031](D:\Nanum-Note\Java\IntelliJ IDEA\IntelliJ IDEA使用总结-img\image-20220720110021031.png)

### 自动打开注释渲染视图

双击`Shift`键并搜索`渲染`：

<img src="D:\Nanum-Note\Java\IntelliJ IDEA\IntelliJ IDEA使用总结-img\image-20220705232945983.png" alt="image-20220705232945983"/>



### 快捷键

- 自动补全

  ![image-20220327174526900](D:\Nanum-Note\Java\IntelliJ IDEA\IntelliJ IDEA使用总结-img\shortcut-02.png)

其他快捷键与当前快捷键冲突请删除其他快捷键，否则自动补全不生效；



### 自动生成作者信息

![image-20220509170224636](D:\Nanum-Note\Java\IntelliJ IDEA\IntelliJ IDEA使用总结-img\image-20220509170224636.png)

**示例模板**

```
/**
 *@author Nanum
 *@since ${YEAR}-${MONTH}-${DAY} ${TIME} 
 */
```



### 注释缩进问题

- XML

    ![image-20220327174252428](D:\Nanum-Note\Java\IntelliJ IDEA\IntelliJ IDEA使用总结-img\zhushi-01.png)

### 代码提示忽略大小写

> 正常情况下Idea中首字母匹配大小写匹配才会出现提示，但是这很不友好，可以改为忽略大小写。

将匹配大小写取消勾选即可。

![image-20220803110738634](D:\Nanum-Note\Java\IntelliJ IDEA\IntelliJ IDEA使用总结-img\image-20220803110738634.png)



# 创建Web工程

- 创建web资源目录：

![image-20220720160920036](D:\Nanum-Note\Java\IntelliJ IDEA\IntelliJ IDEA使用总结-img\image-20220720160920036.png)

- 添加工件：

![image-20220720161142034](D:\Nanum-Note\Java\IntelliJ IDEA\IntelliJ IDEA使用总结-img\image-20220720161142034.png)

- 部署工件：

<img src="D:\Nanum-Note\Java\IntelliJ IDEA\IntelliJ IDEA使用总结-img\image-20220720161247613.png" alt="image-20220720161247613"  />

