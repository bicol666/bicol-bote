# 一、JSP简介

> JSP的全称是Java Servlet Pages，即Java服务器页面，是一种用于开发动态web资源的技术；
>
> JSP的主要作用是代替Servlet回传HTML页面；

**JSP的本质**

JSP页面的本质还是Servlet，第一次访问JSP页面的时候，Tomcat服务器会将JSP页面转译为Java文件，然后编译成字节码文件；



# 二、JSP指令

> JSP指令（directive）是为JSP引擎而设计的，它们并不直接产生任何可见输出，而只是告诉引擎如何处理JSP页面中的其余部分。
>
> Tomcat会根据JSP的指令信息来转译JSP，生成Java源码，生成的Java源码当中不存在指令信息。

**格式**

```jsp
<%@ 指令名 attr1=”” attr2=”” %>
```



- 一般都会把JSP指令放到JSP文件的最上方，**但这不是必须的。**

- JSP中有三大指令：page、include、taglib，最常用，也最为复杂的是page指令；



## 1、page指令

### 1） page指令之pageEncoding&contentType

```jsp
<%@ page language="java" pageEncoding="UTF-8" contentType="text/html; charset=UTF-8" %>  
```



- pageEncoding指定的是当前JSP页面的编码！Tomcat会使用这个编码把JSP转译成Java文件；

- contentType指定的是响应给客户端时使用的编码，等价于`response.setConteType()`；

- 无论是page指令的pageEncoding还是contentType，它们的默认值都是ISO-8859-1，ISO-8859-1是无法显示中文的，所以JSP页面中存在中文的话，一定要设置这两个属性。
- 当pageEncoding和contentType只出现一个时，那么另一个的值与出现的值相同。如果两个都不出现，那么两个属性的值都是ISO-8859-1。



### 2）page指令之import

```jsp
<%@ page import="java.util.*”%>
```

- import属性等价于java当中的`Import`;
- import属性是唯一可以重复出现的属性：



可以使用一条指令同时导入多个包，也可以一条指令导入一个包；

```jsp
方式一：
<%@ page import="java.net.*,java.util.*"%>
----------------------------------------------------
方式二：

<%@ page import="java.util.*"%>

<%@ page import="java.net.*"%>
```



### 3）page指令之errorPage和isErrorPage

```jsp
<%@ page errorPage="xxx.jsp"%>
```

- 在一个JSP页面出错后，Tomcat会响应给我用户错误信息！如果不希望Tomcat给用户输出错误信息，那么可以使用page指令的errorPage来指定错误页;

- 当JSP页面出现错误时，会转发到xxx.jsp页面。



### 4）page指令之isELIgnored

page指令的isElIgnored属性表示当前JSP页面是否忽略EL表达式，默认值为false，表示不忽略。

# 三、JSP脚本

## 1、小脚本

**格式：**`<% java代码 %>`

**作用：**进行逻辑判断

 **特点**:
局部代码块中声明的java代码会被原样转译到jsp对应的servlet文件的_JspService方法中;

代码块中声明的变量都是局部变量。

**ps：**不常用，书写麻烦，阅读困难，被JSTL表达式代替；



## 2、表达式脚本

**格式：**` <%= 要输出的内容 %> `，等价于out.print();

**作用：**往HTML页面输出内容，等价于Servlet中的resopnse.getWriter.print();

**ps：**不常用，书写麻烦，阅读困难，被EL表达式代替；



## 3、声明式脚本

**格式：**`<%! java代码%>`

**作用：**在类中定义全局成员变量和静态代码块；



# 四、JSP静态引入&动态引入

### 1、静态引入

**格式：**`<%@include file="要引入的jsp文件的路径”%>`

**特点：**
会将引入的jsp文件和当前jsp文件转译成一个java(Servlet)文件。在网页中显示了合并后的显示效果。
**注意;**
静态引入的isp文化不会单独转译成java(Servlet)文件。
当前文件和静态引入的jsp文件中不能够使用java代码声明同名变量；



### 2、动态引入

**格式：**

**特点：**动态引入的JSP页面会转译成一个单独的servlet；

**注意：**要引入的jsp页面中不可以使用当前jsp页面中的变量；



# 五、JSP转发标签forward

```jsp
<jsp:forward page="要转发的jsp文件的路径"></jsp:forward>
```

在转发标签中间除了写`<jsp:param name="xxx" value="xxx”/>`子标签不会报错，其他任意字符都会报错。
name属性为数据的键名，value为数据内容；

JSP会将数据以?的形式拼接在转发路径的后面。


# 六、JSP注释

- HTML标签注释：会被转译成普通字符，浏览器解析时识别到这是HTML注释，不会执行；
- Java语言注释：会被转译成普通字符，不会被servlet执行；
- JSP注释：不会被转译
