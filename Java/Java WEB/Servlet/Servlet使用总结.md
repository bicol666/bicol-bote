# Servlet.md

# B/S结构

![image-20211025205729603](C:\Users\nanum008\AppData\Roaming\Typora\typora-user-images\image-20211025205729603.png)

# 一个web应用的目录结构

- webapp：web项目的根目录；
  - WEB-INF
    - classes：存放java文件编译后的字节码文件；
    - lib（可选）：存放第三方的jar包；
    - web.xml：web应用的核心配置文件；
    - 其他静态资源文件/目录（可选）
  - 其他静态资源文件/目录【HTMl、CSS、JS、音视频资源文件】（可选）

PS：其他的资源文件如果存放在web项目的根目录之下，用户可以直接通过URL（项目的根目录名+静态资源路径）访问。而存放在**WEB-INF**下的静态资源文件则用户无权限访问，这里面的静态资源文件“只能由Servlet返回”；

# 关于JavaEE的版本改动

- JavaEE最高的版本是JavaEE8；

- JavaEE目前被Oracle捐献了（开源）给了Eclipse基金会，而Eclipse基金会将JavaEE改名为**jakarta EE**，JavaEE8以后的版本都叫做**jakartaEE**。所以JavaEE8升级以后的“JavaEE9”应该叫做**jakarataEE9**。

  

- Eclipse软件基金会在JavaEE9时将JavaEE的命名空间进行了迁移：**javax.*** --->**jakarta.***，也就是说对软件包进行了改名；并且 所有模块大版本号+1，如： Servlet 4.0.2 -> Servlet 5 以表示其断层式升级；

  

- Tomcat10.+需要使用jakartaEE（ jakarta.* ），而Tomcat10以前的版本可以使用JavaEE（ javax.* ）；

  

  **可能会导致的问题**：

  当我们使用的Tomcat是10以下的版本，而导入的jar包是最新的**jakarta.servlet-api**项目部署后会发现Servlet配置正确，但是Tomcat就是给我们报**404**；

  **原因**：这是由于版本10以下的Tomcat使用的是 *javax.srvlet* 这个软件包（maven坐标的ArtifactId是：javax.servlet-api），而我们项目中导入的是最新的 *jakarta.srvlet* 这个软件包；

  这会导致我们写的Servlet会用 *jakarta.servlet* 这个软件包来编译，而当用户访问Servlet时，Tomcat会去找 *javax.servlet* 的子类，这肯定会找不到；

  **解决办法**：Tomcat的版本是10以下时：项目导入 *javax.servlet-api* ，版本是10及以上时：项目导入 *jakarta.servlet-api* ；



# Maven坐标

*javax.servlet*最高版本是4.0.1

```xml
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
            <scope>compile</scope>
        </dependency>
```

*javax.servlet*现在迁移到了*jakarta.servlet*，所以servlet最新版本在这个坐标中：

```xml
        <dependency>
            <groupId>jakarta.servlet</groupId>
            <artifactId>jakarta.servlet-api</artifactId>
            <version>5.0.0</version>
            <scope>compile</scope>
        </dependency>
```



# Servlet

> Servlet是一个接口，我们负责开发这个接口的实现类，而像**tomcat**这样的web服务器负责根据这套接口调用我们开发的实现类的方法来处理WEB请求；



### 继承关系

> interface jakarta.servlet.**Servlet**

- 唯一子类（实现类）：abstract class	jakarta.servlet.**GenericServlet**

  - 不推荐直接实现Servlet接口；

  - Servlet中的有一些方法我们用不到，核心的方法其实就只有**srevice()**而已，对于其他方法我们大多数情况下不会用到，如果直接实现Srevlet接口的话代码非常难看，不简洁优雅，于是就有了**GenericServlet**。它是一个通用的Servlet，采用了适配器模式设计，将我们不常用的方法（init、destroy……）实现（封装）了，只有一个抽象方法*service()*来让子类实现。

  - 源码分析

    ```java
    public abstract class GenericServlet implements Servlet, ServletConfig, Serializable{
        private transient ServletConfig config;
        
        //	这些方法被GenericServlet实现了（只是空实现），子类如果没有需求的话就不用去实现（重写）；
        public void destroy() { }
        public String getServletInfo() { return "";}
        
       /**
         * 在init()方法中将WEB容器构造好的Servlet实例赋值给了全局变量config，同时在实现的方法：			getServletconfig()当中返回config，这样我们编写的子类可以通过getServletConfig来获取配置信息；
         *  然后调用了GenericServlet扩展的init()方法，这个扩展的方法是为了让我们有机会在Servlet实例化时编写代码，只需重写这个方法即可；
         *  因此千万不要重写init(ServletConfig config)这个方法，否则会导致ServletConfig获取不到，从而导致像getServletConfig()、getInitParameter()、getInitParameters()这样的依赖于ServletConfig对象的方法失效；
         */
        public void init(ServletConfig config) throws ServletException {
            this.config = config;
            this.init();
        }  
       public ServletConfig getServletConfig() {
            return this.config;
        }
        public void init() throws ServletException {}
    
        //调用ServletConfig的getInitParameter()
        public String getInitParameter(String name) {
        }
        //调用ServletConfig的getInitParameters()
        public Enumeration<String> getInitParameterNames() {
        }
    }
    ```
  
  - 唯一子类：class  jakart.servlet.http.**HttpServlet**
  
  

### 主要方法

- **void init(ServletConfig servletConfig)**

  ​	在Servlet实例化后立即被调用，用于执行一些初始化操作，比如：初始化数据库连接池、线程池……

  init方法和构造方法执行时间几乎一致，功能也类似，但是为什么还要有一个init方法呢？

  ​	因为编写Servlet类的时候不建议程序员手动添加构造方法，这可能导致在写有参构造的时候忘记添加无参构造从而导致WEB服务器实例化实例化Servlet异常，WEb服务器是通过反射机制以及无参构造来创建Servlet的，所以init方法是有必要存在的；

  ​	该方法只会被调用一次，不常用；

- **void service(ServletRequest servletRequest, ServletResponse servletResponse)**

  Servlet核心方法，当用户发起web请求时，WEB服务器会调用这个方法来处理用户请求；

  该方法主要负责：获取请求参数、处理请求、响应结果；

- **void destroy()**

  Servlet实例被销毁之前会被调用，可以在这里对资源（比如：IO流、数据库连接……）进行释放；

  只会被调用一次，不常用；

- **ServletConfig getServletConfig()**

- **String getServletInfo()**

  返回这个Servlet的相关信息：作者、编写时间……

  实际开发基本不用；

### Servlet的定义

想要定义一个Servlet需要以下步骤：

- 实现`Servlet`接口或者继承其子类：`jakarta.servlet.GenericServlet`、`jakarta.servlet.http.HttpServlet`；
- 如果是实现的Servlet接口：实现接口内部的抽象方法；
- 如果是基于GenericServlet的扩展类：重写service方法；



### 注册Servlet

> 定义好的Servlet需要注册才能生效，才能被Tomcat这样的WEB服务器识别；



**方式一：web.xml文件里注册**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
         version="5.0">  
    
    一个Servlet标签对应一个Servlet类；
    <servlet>
        Servlet的别名，便于servlet-mapping标签与当前Servlet进行绑定；
        <servlet-name>forwardServlet</servlet-name>
        Servlet类的`全限定类名`；
        <servlet-class>com.nanum.ForwardServlet</servlet-class>
        servlet实例被创建时可以获取这里的初始化参数，K-V形式（可选项）；
        <init-param>
            <param-name>name</param-name>
            <param-value>value</param-value>
        </init-param>
        加上该标签后WEB服务器在启动时以及web应用部署后会 立即 创建Servlet的实例；
        数字表示servlet加载的优先级，数值越小的Servlet，越会被WEB服务器优先创建实例；
		<!--<load-on-startup>1</load-on-startup>-->
    </servlet>

    1、servlet映射标签，用于为servlet配置访问路径，即URL；
    2、url-pattern标签的值的最前面切记加上`/`以表示从当前项目的根目录开始访问；
    3、一个servlet标签至少要有一个servlet-mapping标签与之匹配；
    <servlet-mapping>
        <servlet-name>forwardServlet</servlet-name>
        <url-pattern>/forwardServlet</url-pattern>
    </servlet-mapping>
</web-app>
```

**web.xml**在项目部署到Tomcat时被Tomcat加载，然后Tomcat根据web.xml文件里配置的Servlet信息，通过反射机制来创建Servlet；



**方式二：使用注解注册**

```java
冒号里面的内容就是`url-pattern`的值，也就是当前Servlet的访问地址；
也可以传入数组为当前servlet配置多个访问地址：{"/mainServlet","/other"}
@WebServlet("/mainServlet")
public class MainServlet extends HttpServlet {
}
```

用注解的方式配置servlet是在servlet3.0之后新增的特性，用于简化servlet、Filter、Listener的声明





### Servlet的生命周期

> 当web应用部署到WEB服务器之后，WEB服务器会立即加载并解析web应用中的*web.xml*，获取Servlet的配置信息，但是默认不会立即实例化Servlet。只有当Servlet被访问的时候WEB服务器才会实例化Servlet；
>
> WEB服务器实例化Servlet是通过Servlet的无参构造函数实现的，所以编写Servlet的时候请务必保证提供无参构造函数；
>
> Servlet只会被WEB服务器创建一个实例，所它是单例的；

- 首先WEB服务器在创建Servlet实例之后会调用`init(ServletConfig servletConfig)`方法来初始化Servlet；

  通过参数ServletConfig可以获取像我们写在web.xml里的*init-param*这样的配置信息来完成Servlet的初始化工作；

- 然后这个Servlet实例就在WEB服务器（WEB Server）的内存当中，直到WEB服务器收到Servlet的WEB请求，这时WEB服务器会调用`service()`方法处理请求以及响应结果。每发送一次请求，这个方法就会被调用一次；

- 最后Servlet实例里被销毁时调用`destroy()`方法；

  ​	a）Servlet实例长时间不被访问会被`GC`销毁；

  ​	b）服务器关闭时会销毁所有Servlet实例；



# Servlet注解开发

> Servlet3.0之后推出了Servlet注解，这样可以减小web.xml的体积；
>
> 但是web.xml还是有必要存在的，因为有一些需要变动的信息，不能直接写到java源代码中；

```java
@WebServlet(urlPatterns="/userServlet")
@WebServlet(values="/userServlet")
@WebServlet("/userServlet")
以上几种方式均可
```







# ServletConfig

> ServletConfig的实例封装了Servlet的配置信息，将`<servlet>`标签里的内容封装成ServletConfig；

```xml
<servlet>
    <servlet-name>forwardServlet</servlet-name>
    <servlet-class>com.nanum.ForwardServlet</servlet-class>
    <init-param>
        <param-name>name</param-name>
        <param-value>value</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
```



### 主要方法

- **String getInitParameter(String var1)**

  根据key获取value，根据参数名获取初始化参数值，也就是标签*param-value*；

- **ServletContext getServletContext()**

  获取ServletContext对象；
  
- **String getServletName()**

  获取Servlet的别名，也就是标签*servlet-name*；

- **Enumeration<String> getInitParameterNames()**

  获取所有*param-name*标签的值；

# ServletResponse与ServletResponse

interface	jakarta.servlet.**ServletRequest**	|	Interface	jakarta.srevlet.**ServletResponse**



### ServletRequest

#### 主要方法

- java.io.PrintWriter	**getPrintWriter()**

  - 该方法返回一个**字符输出流**，通过这个流我们就可以向WEB请求输出（响应）字符内容；输出的内容可以是：普通文本、HTML、CSS、JS。

    PS：返回HTML等类型时需要设置响应内容的类型（PrintWriter.setContentType( )），浏览器才知道WEB请求响应的是什么类型的内容;

  - 这个输出流无需程序员刷新（flush）和关闭（close），这些工作由Tomcat工作负责；

- 





### ServletResponse

**常用方法**

| 方法                      | 说明                                   |
| ------------------------- | -------------------------------------- |
| getRequestDispatcher()    | 获取请求调度对象，用作请求转发；       |
| getSession()              | 获取Session对象；                      |
| getCookies()              | 获取所有的Cookie，返回一个Cookie数组； |
| getParameter(String name) | 根据参数名获取请求行参数；             |
| getHeader(String name)    | 根据参数名获取请求头参数             |
| getHeaderNames()              | 获取所有的请求头参数名；                 |
| getRemoteAddr()               | 获取远程ip地址；                         |
| getRemoteHost()               | 获取远程主机的地址；                     |
| getRemotePort()               | 获取远程主机的端口号；                   |
| getProtocol()                 | 获取请求所使用的协议；                   |







# ServletContext

> 一个ServletContext对应一个项目，是整个webapp的上下文环境(应用域)；



## 1、获取方式

1）从Servlet获取，所有的Servlet共享一个ServletContext；

2）从Servletonfig对象获取；

3）从HttpServletRequest对象获取；



## 2、常用方法

| 常用方法                                   | 说明                                                         |
| ------------------------------------------ | ------------------------------------------------------------ |
| String  getInitParameter(String name)      | 获取web.xml里的*context-param*                               |
| String  getRealPath(String  path)          | 获取当前web应用下的文件在磁盘上的绝对路径；可以加上斜杠      |
| String  getContextPath()                   | 获取当前项目的根目录；                                       |
| getResourceAsStream(String path)           | 获取当前项目中的某个文件的流对象；                           |
| void  log(String msg)                      | 输出日志；输出的日志记录到tomcat的logs目录下；               |
| void  setAttribute(String name,Objet vale) | 在整个webapp上下文环境中添加属性，所有用户共享；只建议添加少量的数据，因为添加大量的数据的话会占用大量堆内存，这个对象的生命周期比较长；大数据两会影响服务器的性能；占用内纯较小的数据，很少修改的，可以放在这个对象里面； |
| void  getAttribute(String name)            | 根据属性名从应用域获取属性；                                 |
| void  removeAttribute(String name)         | 根据属性名从应用域删除属性；                                 |





# HttpServlet

> HttpServlet类是专门为HTTP协议准备的。比GenericServlet更加适合HTTP下的开发；



### 继承关系

**父类：**abstract class	jakarta.servlet.**GenericServlet**

### 常用方法

- void	**service**(ServletRequest request,ServletResponse response)

  重写的父类service方法，会将request和response向下转型为HttpServletRquest和HttpServletResponse，然后调用重载的service()方法；

- void	**servie**(HttpServletRequest  request,HttpServletResponse respponse)

  从request中获取到请求的方式（request.getMethod）后调用不同的方法处理不同类型的请求；

  采用模板方法设计模式，指只定义了核心的算法骨架，没有具体的逻辑实现，交由子类去实现；

- void	**doGet**(HttpServletRequest  request,HttpServletResponse respponse)

  默认调用*sendMethodNotAllowed()*来报405错误，因为子类没有重写doGet的话，这个servlet是不支持GET请求方式的；

  z这个方法主要是交由子类去重写实现具体的逻辑，处理GET请求的；

- void	**doPOST**(HttpServletRequest  request,HttpServletResponse respponse)

  与*doGet()类似*；


### HttpServletRequest与HttpServletResponse

> 这两个类是对Http的请求报文与响应报文的封装；
>
> 这两个类由WEB服务器创建；

#### 生命周期

这两个对象的生命是一次Http请求，请求响应结束后这两个对象就会被销毁；



#### HttpServletRequest

interface	jakarta.servlet.http.**HttpServletRquest**

**主要方法**

- String	**getMethod**();

  返回Http请求的方式，GET、POST、PUT……

- String    **getParameter(String  name)**

  根据参数名获取参数值；

- Map<String,String[]>    **getParameterMap()**

  HttpServletRequest内部采用Map<String,String[]>来存储参数，value是String[]是因为可能有多个参数，参数名一样，值不一样，这样用String数组就可以存下了；

- String   **getParameter(String name)**

  根据参数名获取参数值（获取value这个字符串数组的第一个元素）；

- String[]    **getParameters(String name)**

  根据参数名获取所有的参数值，因为有可能存在多个同名参数的情况；

- void    **setAttribute(String name,Object value)**

  在请求域添加属性（数据），多用于转发；

- RequestDispatcher    **getRequestDispatcher(String  path)**

  获取一个请求转发器，参数是转发的路径；

- String    **getRemoteAddress()**

  获取客户端的IP地址；

- void    **setCharracterEncoding(String encode)**

  设置请求体的字符集，处理POST请求中文乱码的问题；

  tomcat10以后，请求体默认的字符集就是*UTF-8*，不会出现乱码的问题；

- String    **getRequestURI()**

  获取请求的URI，从项目根目录开始；

- HttpSession    **getSession()**

  获取session对象，获取不到则新建一个session；







#### HttpServletResponse

响应乱码问题解决：response.setContentType("text/html;charset=UTF-8")





# 欢迎页的配置

> 欢迎页可以是静态页面，也可以是servlet；

- 局部配置：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
         version="5.0">  
    这里配置的欢迎页是局部配置，只对当前项目有效；
    这里也可以填servlet的URL
    <welcome-file-list>
        <welcom-file>login.html</welcom-file>
    </welcome-file-list>
</web-app>
```

- 全局配置：

在：tomcat根目录/conf/web.xml里进行配置，默认是：index.html、index.htm、index.jsp；

原则：局部优先；

# 转发与重定向

## 1、转发

Servlet转发就是将一次Http请求转发给另一个Servlet处理，浏览器总共还是一次请求；

```java
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //转发时可以在HttpServletRequest对象当中携带属性(数据)；
        req.setAttribute("user",new User());
        //将请求转发给otherServlet处理；
        req.getRequestDispatcher("otherServlet").forward(req,resp);
    }
```



注意：

转发可能会引起：`405方法不允许`

假设AServlet的的一个post请求转发给BServlet处理，而Bservlet又只重写了`doGet()`方法，只能处理get请求。这时post请求由AServlet转发给BServlet，BServlet又不能处理post请求就会报405错误。所以，对于转发与重定向请合理选择；



## 2、重定向

重定向就是响应给浏览器，告诉浏览器再往服务器发一次请求，一共是两次请求；



# Cookie

删除Cookie

cookie.setMaxAge(0):这表示cookie的失效时间为立马失效，就等于删除Cookie；



获取Cookie

Cookie[]	request.getCookies()

注意：如果浏览器未提交Cookie那么这个方法获取到的结果是NUll，而不是一个空数组；

# BS结构的Session机制

### 什么是session？

- session是会话的意思；
- 用户打开浏览器后打开网站，到关闭浏览器是一次会话；

- 一次会话对应N次请求；
- session机制属于BS结构中的一部分，使用其他语言开发WEB项目也会有session机制；
- session最主要的作用就是保存会话的状态信息,例如登录信息……

### Java中的Session

jakarta.servlet.http.HttpSession

我们可以往session里面放数据（session.setAttribute(String ,Object)）；



配置Session超时

<session-timeout>超时时间</session-timeout>



cookie禁用解决办法：URL重写机制

就是在URL的末尾加上：`;jsessionid=XXXXXXXXXXXXXXXX`，服务器就能识别这个会话了；

# 拦截器

使用注解`@WEBFilter`的时候，所拦截的路径最前方一定要是是`/`，否者部署工件会报错；
