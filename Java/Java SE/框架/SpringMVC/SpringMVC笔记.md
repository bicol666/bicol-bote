# 一、简介

# 二、依赖坐标

**Gradle**

```groovy
ext {
    junitVersion = '5.7.1'
    springVersion = '5.3.12'
}

dependencies {
    //单元测试
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.7.0'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.2'
    

    //spring
    implementation("org.springframework:spring-context:${springVersion}")
    implementation("org.springframework:spring-jdbc:${springVersion}")
    implementation("org.springframework:spring-aspects:${springVersion}")
    implementation("org.springframework:spring-webmvc:${springVersion}")

    
    //mybatis
    implementation 'org.mybatis:mybatis:3.5.7'
    implementation 'org.mybatis:mybatis-spring:1.3.2'
    implementation 'mysql:mysql-connector-java:8.0.27'
    implementation 'com.alibaba:druid:1.2.6'
    implementation 'com.github.pagehelper:pagehelper:3.4.2'
    
    
    //jstl
    implementation 'jstl:jstl:1.2'
    implementation 'taglibs:standard:1.1.2'

    //lombok
    implementation 'org.projectlombok:lombok:1.18.22'

    //servlet
    compileOnly('javax.servlet:javax.servlet-api:4.0.1')
    
    //日志记录
    implementation 'log4j:log4j:1.2.17'
    
    //fastjson
    implementation 'com.alibaba:fastjson:1.2.78'

}
```



**Maven**

```xml
<!-- 统一版本管理 --> 
<properties>
        <maven.compiler.source>16</maven.compiler.source>
        <maven.compiler.target>16</maven.compiler.target>
        <spring.version>5.3.12</spring.version>
    </properties>
    <dependencies>
        <!-- Spring -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!--  Spring对jdbc的支持 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!-- SpringMVC -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!--SpringAOP-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>${spring.version}</version>
        </dependency>
        
        <!--Junit单元测试-->
	<dependency>
   	 <groupId>junit</groupId>
  	  <artifactId>junit</artifactId>
  	  <version>4.13.1</version>
	    <scope>test</scope>
	</dependency>


         <!--MyBatis-->
		<dependency>
   		  <groupId>org.mybatis</groupId>
	     <artifactId>mybatis</artifactId>
         <version>3.5.7</version>
		</dependency>
        
        <!--Mybatis对Spring的整合支持-->
         <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>1.3.2</version>
        </dependency>
        
        <!--MySQL连接驱动-->
	<dependency>
  	  <groupId>mysql</groupId>
 	   <artifactId>mysql-connector-java</artifactId>
  	  <version>8.0.27</version>
	</dependency>

    </dependencies>

```



# 三、web.xml配置

> Spring-MVC 是通过Servlet启动，所以需要在web.xml中配置SpringMVC的Servlet

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

<!--配置中央处理器Servlet-->
    <servlet>
        <servlet-name>DispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
			<!-- 配置 spring配置文件的路径-->
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:AppContextConfig.xml</param-value>
        </init-param>
		<!-- 配置servlet的优先级：在tomcat启动的时候就创建servlet-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
        <!--  /代表拦截所有的请求(jsp文件除外)，/*会拦截所有的请求交给controller处理-->
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    
    	<!-- 统一编码的过滤器 -->
	<filter>
		<filter-name>CharacterEncodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>utf-8</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>CharacterEncodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
    
</web-app>
```



# 四、Spring-MVC配置

## 1、基本配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
		http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd">
    
    <description>Spring MVC Configuration</description>
    
    <!-- 加载Properties文件 -->
	<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
		<property name="locations">
			<list>
				<value>classpath:config.properties</value>
			</list>
		</property>
	</bean>
    
    <!-- 使用Annotation自动注册Bean,只扫描@Controller -->
	<context:component-scan base-package="cn.tyrone.wallet.controller" use-default-filters="false">
		<context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
	</context:component-scan>
    
    <!-- 定义视图文件解析 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="${web.view.prefix}"/>
		<property name="suffix" value="${web.view.suffix}"/>
	</bean>
	
	<mvc:annotation-driven />
	
	<!-- 静态资源映射 -->
    <span style="white-space:pre">	</span><mvc:resources mapping="/static/**" location="/static/" cache-period="31536000"/>
	
	<!-- 定义无Controller的path<->view直接映射 -->
	<mvc:view-controller path="/" view-name="redirect:${web.view.index}"/>
    
</beans>    
```



## 2、静态资源放行

> SpringMVC的`DispatcherServlet`会拦截所有的请求，如果UrlPatern是`/*`表示拦截所有请求交给Controller处理，如果UrlPatern是`/`表示拦截除了`isp`以外的所有请求交给Controller处理；
>
> 如果不对静态资源放行的话，对于jsp以外的请求(静态资源)，服务器会报`404`；



方式一：

```xml
<!--location:文件实际路径。 mapping：请求的URL地址 -->
<!--这表示将所有的js、css请求路径转移到WEB-INF下处理-->
<mvc:resources location="/WEB-INF/js/" mapping="/js/**" />
<mvc:resources location="/WEB-INF/css/" mapping="/css/**" />
```



方式二：使用默认的Servlet处理

```xml
<mvc:default-servlet-handler />
```



注意：两种方式不可混合使用，否者路径会有很大问题；

# 五、Controller

> 创建一个类，使用@Controller注解对该类进行标识，这个类就是SpringMVC中的控制器类了;
>
> @Controller有两层含义：1、该类交给Spring管理；2、语义化，控制层类；
>
> @RequestMapping("url")：配置当前控制器的请求路径；



## 1、Controller接收前端请求参数



**方式一：**使用注解：`@RequestParam`；

```java
    @RequestMapping("/register")
    public void test(@RequestParam("username") String username, @RequestParam("password") String password,@RequestParam("email") String email, @RequestParam("interest")String[] interest) {
        System.out.println("username = " + username);
        System.out.println("password = " + password);
        System.out.println("email = " + email);
        System.out.println("interest = " + Arrays.toString(interest));
    }
```

使用注解@RequestParam("参数名") 即可接收前端传递的文本类型参数，当同一参数有多个值(复选框)可以用数组接收；

**ps：当前端的参数和形参名字一致的时候注解可以省略不写；**



**方式二：**使用对象接收；

> 在形参定义一个自定义类型的参数，SpringMVC可以将我们的前端参数直接封装成一个对象；

User.java

```java
@Data
public class User {
    private String username;
    private String password;
    private String email;
    private String[] interest;
}
```

UserController.java

```java
    @RequestMapping("/register1")
    public void test(User user) {
        System.out.println("user = " + user);
    }
```



## 2、Controller接收请求头参数

> 接收前端参数使用注解：`@RequestHeader("参数名")`；
>
> 想要接收请求头参数，不管请求头参数名与定义的形参名是否一致，都不能省略这个注解；

```java
    @RequestMapping("/register1")
    public void test(User user, @RequestHeader("Referer") String Referer) {
        System.out.println("user = " + user);
        System.out.println("Referer = " + Referer);
    }
```



## 3、Controller接收JSON字符串

> 使用注解@RequestBody，SpringMVC会自动帮我们将JSON字符串封装成一个对象；

```java
    @RequestMapping("/register1")
    public void test(@RequestBody User user) {
        System.out.println("user = " + user);
    }
```

**注意：SpringMVC进行JSON转换的时候，依赖的是`jackson`，所以要导入`jackson`的依赖；**

**jackson依赖坐标：**

**Maven**

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.13.0</version>
</dependency>

```

**Gradle**

```groovy
implementation 'com.fasterxml.jackson.core:jackson-databind:2.13.0'
```





# 六、文件上传

文件上传实例：



## 1、依赖坐标

> 文件上传需要依赖`common-io`和`common-fileupload`这两个jar包；

**Gradle：**

```groovy
```



**Maven：**

```xml
```



## 2、配置文件解析器

> 在Spring容器创建文件解析器Bean；

```xml
```



## 3、form表单配置

```html
<from>
    <input	type="file" name="文件项">
</from>
```





## 4、Controller接收文件

