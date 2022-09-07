# IDEA开发web应用总结.md

# 新建一个web项目

步骤一：新建一个普通的项目；

步骤二：在项目结构设置里选择`Facet`添加WEB模块；



# webapp目录以及web.xml的配置

![image-20220314142645149](D:\Nanum-Note\Java\JavaWeb\Servlet\IDEA开发WEB应用总结\img\webapp01.png)

这两个配置对于构建web工件来说是必须的，路径没配置好，可能工件部署后导致项目访问出错；



# IDEA控制台输出中文乱码

> 控制台输出的中文之所以乱码，是因为Tomcat对控制台输出的编码方式与IDEA控制台的编码方式不一致导致的；

### **配置Tomcat控制台日志输出编码方式**

步骤一：打开配置文件

Tomcat根目录--->conf--->**logging.properties**，这个文件就是对Tomcat日志输出的配置文件。

步骤二：修改编码方式

`java.util.logging.ConsoleHandler.encoding = UTF-8`

这一行代码就指定了Tomcat对控制台输出的编码方式；



### **解决乱码**

建议Tomcat与IDEA都使用**UTF-8**，这样就不会乱码了。



配置IDEA控制台编码方式为UTF-8：

![](D:\Nanum-Note\Java\JavaWeb\Servlet\IDEA开发WEB应用总结\img\artifact05.png)

<img src="D:\Nanum-Note\Java\JavaWeb\Servlet\IDEA开发WEB应用总结\img\encoding01.png" alt="image-20220314144202418"  />

在虚拟机选项处添加这句话：`-Dfile.encoding=utf-8`；

# DEA中的工件（Artifact）

### **工件是什么？**

IDEA中的工件就是当前项目（模块）进行构建后输出的结果，如：jar、webapp、war……；



### **如何打开工件设置页面？**

步骤一：打开项目结构设置；

![image-20220314131652580](D:\Nanum-Note\Java\JavaWeb\Servlet\IDEA开发WEB应用总结\img\artifact01.png)

步骤二：选择工件即可；

![image-20220314132015785](D:\Nanum-Note\Java\JavaWeb\Servlet\IDEA开发WEB应用总结\img\artifact02.png)



### 如何添加工件？

步骤一：点击加号按钮；

![image-20220314132306559](D:\Nanum-Note\Java\JavaWeb\Servlet\IDEA开发WEB应用总结\img\arifact03.png)



步骤二：这里列出了所有类型的工件，开发WEB应用的话选择框出来的两种，最后选择我们要构建的模块即可；

**展开型和归档型的区别：**

- 展开型（exploded）

  展开型就是一个符合web应用目录结构的文件夹，也就是web项目没有打成war包之前的样子；

- 归档型（war）

  顾名思义，就是打成war包，选择这种工件部署后我们的Tomcat的*weapps*目录下还会生成一个web应用（文件夹）。

  **归档（war）是对于展开型的工件进行归档。因此先有展开型的工件，才能添加与之对应的归档型的工件；**

  推荐使用这种工件，因为我们可以在Tomcat下查看当前项目构建的web应用生成了那哪些内容；

![image-20220314132730902](D:\Nanum-Note\Java\JavaWeb\Servlet\IDEA开发WEB应用总结\img\artifact04.png)



### 工件的输出目录

工件输出的目录默认是项目根目录下的*out*目录下的**artifacts**；



### 部署工件

步骤一：打开*编辑配置*

![image-20220314134605373](D:\Nanum-Note\Java\JavaWeb\Servlet\IDEA开发WEB应用总结\img\artifact05.png)

步骤二：部署工件

![image-20220314135423127](D:\Nanum-Note\Java\JavaWeb\Servlet\IDEA开发WEB应用总结\img\artifact06.png)



### 构建工件（打包）

> 构建工件就是打包；

