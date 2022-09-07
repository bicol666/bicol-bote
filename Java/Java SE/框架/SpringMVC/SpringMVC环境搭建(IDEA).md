# 一、新建项目

![image-20211126173129641](C:\Users\nanum008\AppData\Roaming\Typora\typora-user-images\image-20211126173129641.png)

![image-20211126173532295](C:\Users\nanum008\AppData\Roaming\Typora\typora-user-images\image-20211126173532295.png)

之后`无脑下一步`即可创建web项目；

# 二、相关依赖坐标

**Maven：**

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



**Gradle：**

```groovy
```



# 三、web.xml配置

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

</web-app>
```

# 四、Spring-MVC配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context" 
	xmlns:jdbc="http://www.springframework.org/schema/jdbc"  
	xmlns:jee="http://www.springframework.org/schema/jee" 
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:aop="http://www.springframework.org/schema/aop" 
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xmlns:util="http://www.springframework.org/schema/util"
	xmlns:jpa="http://www.springframework.org/schema/data/jpa"
	xsi:schemaLocation="
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.2.xsd
		http://www.springframework.org/schema/jdbc http://www.springframework.org/schema/jdbc/spring-jdbc-3.2.xsd
		http://www.springframework.org/schema/jee http://www.springframework.org/schema/jee/spring-jee-3.2.xsd
		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-3.2.xsd
		http://www.springframework.org/schema/data/jpa http://www.springframework.org/schema/data/jpa/spring-jpa-1.3.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-3.2.xsd
		http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-3.2.xsd
		http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-3.2.xsd">
	<!-- 配置注解扫描范围 -->
	  <!-- 只扫描控制器 -->
    <context:component-scan base-package="com.nanum.test" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
    
	<!-- 配置扫描MVC注解 -->
	<mvc:annotation-driven />
    
	<!-- 配置视图解析器， -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/" /><!-- 前缀 -->
		<property name="suffix" value=".jsp" /><!-- 后缀 -->
	</bean>
	
	<!-- 配置拦截器 -->
	<mvc:interceptors>
		<mvc:interceptor>
			<mvc:mapping path="/*"/>
			<bean class="interceptors.SomeInterceptor"/>
		</mvc:interceptor>
	</mvc:interceptors>
</beans>
```

# 五、Spring配置
