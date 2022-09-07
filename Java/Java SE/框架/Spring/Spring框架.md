# Spring框架

## 一、Spring-IoC

## 1、IoC&DI

IOC(Inversion Of Control)是控制反转的意思，它不是一种技术实现，而是一种设计思想，一个重要的面向对象编程的法则，它能指导我们设计出低耦合的软件系统;

DI(Dependency Injection)是依赖注入的意思，是实现IOC的一种方式；

**问题诞生背景：**

在面向对象设计的软件系统当中，底层都是由无数个对象构成的，每个对象之间相互合作来实现我们的业务需求；

但是随着我们的业务需求的复杂度的提升，我们的软件系统系统也逐渐变得庞大，底层的对象非常多，对象之间的依赖关系异常复杂(耦合度高);`耦合度高`就会导致当我们的需求变动的时候`牵一发而动全身`有可能简单的变动都要花大功夫去改源码，项目的开发与维护变得异常困难,这个时候就需要一种方法(思想)来解耦合；



**对IOC的理解**

<img src="https://img-blog.csdnimg.cn/20200420115748990.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9mZW5nMTAzMzAxMTE=,size_16,color_FFFFFF,t_70" style="zoom:50%;" />

既然是`控制反转`，那么我们就应该弄清楚：`谁控制谁,控制了什么？`、`又反转了什么？`、`既然有反转，那么正转又是什么呢？`……

- 谁控制谁，控制什么：在传统Java程序当中，我们是直接在对象内部通过new进行创建对象，是对象主动去获取创建`依赖对象`；而IOC是专门有一个容器来负责创建这些对象，即由IOC容器来控制对象的创建、装配以及外部资源获取（不只是对象包括比如文件等）。
- 为何是反转，反转了什么：有反转就有正转，传统应用程序是由我们自己在对象中主动控制去直接创建依赖对象，也就是正转；而反转则是由IOC容器来帮忙创建及注入依赖对象：由容器帮我们查找及注入依赖对象，对象只是被动的接受依赖对象，所以是反转，`依赖对象的创建方式`被反转了。



**对DI的理解**

- DI是依赖注入的意思，即通过属性的`set`方法结合反射机制给对象的属性赋值；



## 2、依赖坐标

> 只需导入spring-context，spring的其他依赖会被gradle自动导入；



Gradle依赖：

```groovy
implementation 'org.springframework:spring-context:5.3.9'
```



Maven坐标：

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.9</version>
</dependency>
```





## 3、创建bean的三种方式

> Spring创建Bean对象的方法一共有三种；
>
> 最常用的是通过set方法创建；

#### 3.1、通过构造方法创建

1、通过无参构造方法创建

```java

```

2、通过有参构造方法创建

```java

```



#### 3.2、通过set方法创建

#### 3.3、通过工厂方法创建



4、属性



## 5、Spring-IoC：基于xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
</beans>
```



2.3 创建SpringBean

```xml
<beans	……>
    <!--一个bean标签就代表一个spring对象，class属性值为全限定类名，id属性是该bean的全局唯一标识-->
    <bean id="User" class="com.nanum.User"/>
</beans>
```

注意：

-  bean 的 id是严格区分大小写的 如果id不存在会报错： NoSuchBeanDefinitionException    
- 如果容器中同一个类型的bean有多个，再根据类型获取的话就会报错： NoUniqueBeanDefinitionException 
- bean一般需要无参构造，spring会调用它来创建对象
- 如果没有无参构造，需要给构造方法的参数赋值：    



### 为创建的Bean属性赋值(DI)

---

为属性赋值的三种方式：

- setter方法
- 有参构造方法
  - 参数的下标
  - 参数的类型

#### 1）通过setter为对象属性赋值



#### 2）通过有参构造函数为对象属性赋值



### Spring整合MyBatis

---

1）导入相关依赖

- Gradle相关依赖：

```groovy
 	//MyBatis相关
    //MySQL驱动
    implementation 'mysql:mysql-connector-java:8.0.25'
    //mybatis
    implementation 'org.mybatis:mybatis:3.5.7'
    //MyBatis-Spring
    implementation 'org.mybatis:mybatis-spring:2.0.6'
    //Druid
    implementation 'com.alibaba:druid:1.2.6'
    //MyBatis分页插件(可选项)
    //implementation 'com.github.pagehelper:pagehelper:5.2.0'

	//Spring相关
    //Spring
    implementation 'org.springframework:spring-context:5.3.9'
    implementation 'org.springframework:spring-jdbc:5.3.9'

    //Servlet
    compileOnly('javax.servlet:javax.servlet-api:4.0.1')

    //单元测试
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1'
```

- Maven相关坐标：

```xml
<!--Junit单元测试-->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13</version>
    <scope>test</scope>
</dependency>

<!--MyBatis-->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.5</version>
</dependency>

<!--MyBatis-Spring-->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.5</version>
</dependency>

<!--MySQL数据库驱动-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.21</version>
</dependency>

<!--Spring-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.12</version>
</dependency>

<!--Lombok(选用)-->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.12</version>
</dependency>
```





2）ApplicaitonContext.xml配置

IoC容器创建Mybatis相关bean：连接池对象、SqlSessionFctory对象、SqlSession对象

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
		http://www.springframework.org/schema/mvc
		http://www.springframework.org/schema/mvc/spring-mvc-3.2.xsd
		http://www.springframework.org/schema/context
		http://www.springframework.org/schema/context/spring-context-3.2.xsd
		http://www.springframework.org/schema/aop
		http://www.springframework.org/schema/aop/spring-aop-3.2.xsd
		http://www.springframework.org/schema/tx
		http://www.springframework.org/schema/tx/spring-tx-3.2.xsd">

    <!-- 读取配置(properties)文件 -->
    <context:property-placeholder location="DruidDataSourceConfig.properties"/>

    <!-- 配置数据库连接池 -->
    <bean id="dataSource" class="${type}">
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="initialSize" value="${initialSize}"/>
        <property name="minIdle" value="${minIdle}"/>
        <property name="maxActive" value="${maxActive}"/>
        <property name="maxWait" value="${maxWait}"/>
    </bean>

    <!-- 配置SqlSessionFactory对象 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 注入数据库连接池 -->
        <property name="dataSource" ref="dataSource"/>
        <!-- 读取MyBatis配置文件 -->
        <property name="configLocation" value="MyBatisConfig.xml"/>
        <!-- 配置mapper文件的位置 -->
        <property name="mapperLocations" value="classpath:mappers/*Mapper.xml"/>
        <!--为实体包下的所有类起别名-->
        <property name="typeAliasesPackage" value=""/>
    </bean>

    <!-- 扫描dao包中的接口，!!创建dao实现类对象(bean)，存储在容器当中!!，以便对service对象进行属性赋值(自动装配) -->
    <bean id="scannerConfigurer" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <!--扫描哪个包-->
        <property name="basePackage" value=""/>
    </bean>

</beans>
```



DruidDataSourceConfig.properties.xml

```properties
#-----------------------------------------------------------
# 一、基本参数
# ps：不建议直接使用username作为属性名，因为properties文件中username与mysql关键字冲突，造成数据库不能访问(报账号密码出错)；
#     其他属性(字段)随意；
jdbc.username=用户名
jdbc.password=密码
jdbc.driver=com.mysql.cj.jdbc.Driver
# serverTimezone:MySQLServer的时区配置 UTC是统一标准世界时间
# characterEncoding：编码字符集
jdbc.url:jdbc:mysql://127.0.0.1:3306/数据库?useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
#-----------------------------------------------------------
#-----------------------------------------------------------
# 二、druid数据源配置
type=com.alibaba.druid.pool.DruidDataSource
# 连接池配置
# 初始化连接数、最小保留连接数、最大连接数
initialSize=1
minIdle=1
maxActive=20
#-----------------------------------------------------------
# 获取连接等待超时时间
maxWait=60000
#-----------------------------------------------------------
validationQuery=select 'x'
testOnBorrow=false
testOnReturn=false
#-----------------------------------------------------------
# 配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
filters=stat,wall,log4j
# 通过connectProperties属性来打开mergeSql功能；慢SQL记录
connectionProperties=druid.stat.mergeSql=true;druid.stat.slowSqlMillis=5000
#-----------------------------------------------------------
#毫秒检查一次连接池中空闲的连接,
timeBetweenEvictionRunsMillis=3600000
#把空闲时间超过minEvictableIdleTimeMillis毫秒的连接断开, 直到连接池中的连接数到minIdle为止 连接池中连接可空闲的时间
minEvictableIdleTimeMillis=600000
#建议配置为true，不影响性能，并且保证安全性。申请连接的时候检测，如果空闲时间大于timeBetweenEvictionRunsMillis，执行validationQuery检测连接是否有效。
testWhileIdle=false
#是否在自动回收超时连接的时候打印连接的超时错误
logAbandoned=true
#是否自动回收超时连接,生产环境最好去掉，开发环境配置是为了更好的发现问题
removeAbandoned=true
#是否缓存preparedStatement，也就是PSCache。PSCache对支持游标的数据库性能提升巨大，比如说oracle。
poolPreparedStatements=true
#要启用PSCache，必须配置大于0，当大于0时，poolPreparedStatements自动触发修改为true。在Druid中，不会存在Oracle下PSCache占用内存过多的问题，可以把这个数值配置大一些，比如说100
maxOpenPreparedStatements=100
#当出现这个错误connection holder is null的时候，要么关闭自动回收的功能,要么加大这个参数
removeAbandonedTimeout=900000
```



## 6、Spring-IoC：基于注解配置

> Spring不仅可以使用xml文件对其进行配置，也可以直接在类中定义注解，以达到配置spring的目的；
>
> 使用注解配置可以简化开发，没有xml配置那么繁琐；

使用注解配置务必在xml配置文件当中新增以下配置：

```xml
<?xml version="1.0" encoding="UTF-8"?>
 <beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-3.0.xsd">
 
    <!-- 开启注解配置 -->
   <context:annotation-config/>
 
 	<!-- 设置注解的扫描范围-->
	<context:component-scan base-package="要扫描的包" />
 
</beans>
```



Spring中常用的注解有以下：

1. 主键类注解
   1. @Component ：标注一个普通的Spring Bean类；
   2. @Repository：标注数据持久层(DAO)类；
   3. @Service：标注业务逻辑层(Service)类；
   4. @Controller：标注数据展现层(Servlet)类；
   5. @Value：为Bean中的属性赋值；
2. 装配bean时的注解
   1. @Autowired
   2. @Resource



### 常用注解之主键类注解

> @Component、@Repository、@Service、@Controller这几个注解本质上是同一类注解，用法和功能都相同；
>
> 以上注解的区别在于标识组件的类型，具有语义化的作用，便于开发和维护；
>
> @Component可以代替@Repository、@Service、@Controller，因为这三个注解是被@Component标注的；



### 常用注解之装配注解

> @Autowired：属于Springorg.springframework.beans.factory.annotation包下,可用于为类的属性、构造器、						方法进行注值
> @Resource：不属于spring的注解，而是来自于JSR-250位于java.annotation包下，使用该annotation为目标						bean指定协作者Bean。
> @PostConstruct 和 @PreDestroy 方法 实现初始化和销毁bean之前进行的操作



**@Autowired与@Resource的异同点**

**1）相同点**
	@Resource的作用相当于@Autowired，均可标注在字段或属性的setter方法上。
**2）不同点**
	a：提供方 @Autowired是Spring的注解，@Resource是javax.annotation注解，而是来自于JSR-250，J2EE提供，需要JDK1.6及以上。
	b ：注入方式 @Autowired只按照Type 注入；@Resource默认按Name自动注入，也提供按照Type 注入；
	c：属性
		@Autowired注解可用于为类的属性、构造器、方法进行注值。默认情况下，其依赖的对象必须存在（bean可用），不存在会报异常，如果需要改变这种默认方式，可以设置其required属性为false。
还有一个比较重要的点就是，@Autowired注解默认按照类型装配，如果容器中包含多个同一类型的Bean，那么启动容器时会报找不到指定类型bean的异常，解决办法是结合@Qualified注解进行限定，指定注入的bean名称。
		@Resource有两个中重要的属性：`name`和`type`。name属性指定byName，如果没有指定name属性，当注解标注在字段上，即默认取字段的名称作为bean名称寻找依赖对象，当注解标注在属性的setter方法上，即默认取属性名作为bean名称寻找依赖对象。
		需要注意的是，@Resource如果没有指定name属性，并且按照默认的名称仍然找不到依赖对象时，@Resource注解会回退到按类型装配。但一旦指定了name属性，就只能按名称装配了。

​	d：@Resource注解的使用性更为灵活，可指定名称，也可以指定类型 ；@Autowired注解进行装配容易抛出异常，特别是装配的bean类型有多个的时候，而解决的办法是需要在增加@Qualitied进行限定。

# 二、Spring-AOP

> 在软件业，AOP为Aspect Oriented Programming的缩写，意为：面向切面编程，通过预编译方
> 式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个
> 热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑
> 的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高
> 了开发的效率。

### 1、相关依赖坐标

```groovy
 implementation 'org.springframework:spring-aspect:5.3.9'
```

### 2、静态代理



### 3、JDK动态代理&CGlib动态代理



### 4、Spring-AOP：基于xml配置

`ApplicationContext.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">
    <!-- target -->
    <bean id="service" class="com.fq.service.impl.OrderServiceImpl"/>
    <!-- advice -->
    <bean id="advice" class="com.fq.advice.ConcreteInterceptor"/>
    <!-- 配置切面 : proxy-target-class确定是否使用CGLIB -->
    <aop:config proxy-target-class="true">
        <!--
            aop:pointcut : 切点定义
            aop:advisor: 定义Spring传统AOP的切面,只支持一个pointcut/一个advice
            aop:aspect : 定义AspectJ切面的,可以包含多个pointcut/多个advice
        -->
        <!-- 定义切点 -->
        <aop:pointcut id="pointcut" expression="execution(* com.fq.service.impl.OrderServiceImpl.*(..))"/>
        <aop:advisor advice-ref="advice" pointcut-ref="pointcut"/>
    </aop:config>
</beans>
```





### 5、Spring-AOP：基于注解配置

