# 一、环境搭建

## 1、Tomcat的安装

- 访问官网下载Tomcat，官网：[https://tomcat.apache.org](https://tomcat.apache.org)；

- 下载后解压即可完成Tomcat服务器的安装。



## 2、JDK的配置

tomcat的运行需要依赖JDK，所以务必配置环境变量`JAVA_HOME`；



## 3、tomcat环境变量的配置

CATALINA_HOME

环境变量CATALINA_HOME的值就是Tomcat的根路径。然后在path环境变量里添加字符串`%CATALINA_HOME%\bin`，这样在命令行窗口输入`startup`即可快捷启动Tomcat服务器，推荐使用这种方式。



# 二、Tomcat服务器的启动与关闭

## 1、启动Tomcat

在tomcat的根路径下找到bin目录，里面的有一个startup.bat文件，双击即可启动tomcat服务器；



- 检测是否正常启动Tomcat服务器

  浏览器输入**127.0.0.1:8080**回车，如果能正常访问就说明Tomcat服务器正常启动。

  **PS**：tomcat的运行需要JRE，即JAVA运行环境，如果出现一个黑的窗口”一闪而过“很有可能是该电脑没有配置环境变量

​		`JAVA_HOME`导致的；



## 2、关闭Tomcat

同理，在bin目录下找到shutup.bat文件，双击即可关闭tomcat服务器。

**ps**：关闭tomcat服务器不建议直接点击windows窗口的关闭按钮。



- 乱码问题

  如果出现乱码问题在Tomcat根目录下找到`conf`目录，在这个目录下找到`logging.xml`文件，打开该文件找到

  java.util.logging.ConsoleHandler.encodin字段，将等号后面值改为**GBK**即可解决乱码问题。

  

# 三、Tomcat的目录结构

tomcat：

​	1）bin----二进制文件，比如`start.bat`、`shutdown.bat`；

​	2）conf----Tomcat的配置文件；

​	3）wenapps----web项目；

​	4）logs----日志文件；

​	5）work----JSP转译成servlet的`java源码文件`以及编译后的`class字节码文件`；

​	6）temp----Tomcat运行时产生的临时文件；



# 四、标准的WEB项目目录结构

WebContent：

​	a)WEB-INF

​		1）lib----项目运行所依赖的`jar`包；

​		2）classes----当前项目(字节码文件)；

​		3）web.xml----核心配置文件；

​	b）其他静态资源文件，如：html、css、js、image……
