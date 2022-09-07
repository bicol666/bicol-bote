# Spring-IOC 知识总结

# 第一章：概述

## 1、谈谈你对IOC的理解？

- 1、**什么是IoC？**
  - IoC-Inversion【[ɪnˈvɜːʃ(ə)n]】 of Control，中文释义：控制反转。
  - 在传统的面向编程开发中，当我们在A类中要用到B类时，需要在A类中显式的`new`一个B类的对象出来。使用`IoC`思想来设计后，我们只需从`IoC容器`中来获取B类的对象即可，无需手动显式的`new`对象。
  - IoC不是什么开发技术，而是面向对象编程【oop】中的一种设计思想或原则。它能指导程序员设计出`低耦合`的优良程序。
    - `低耦合`体现在：传统应用程序中我们是手动在类的内部创建依赖对象，当应用程序的体系大到一定程度后，类的数量越来越多，类多了之后类与类之间的依赖关系就变得异常复杂，耦合度就变高，就像一团乱麻搅在一起。想要理清类之间的关系非常麻烦，不利于我们对应用程序进行扩展和维护。
    - 当采用`IoC`思想来设计后，对象只需从IoC容器获取依赖对象即可，所有对象由IoC容器统一管理。对象与依赖对象之间不产生直接关系，而是与IoC容器产生直接关系。



- 2、**IoC反转了什么？**

  **获取依赖对象的过程被反转了。**原本由对象主动获取、创建依赖对象的过程变成了由`IoC容器`为对象注入依赖对象。对象从主动获取依赖对象变成了被动接受。

- 3、**DI与IoC的关系？**

  DI-Dependency Injection【依赖注入】，依赖注入指的是IoC容器在运行时动态地将依赖对象注入到对象中。其实它们是同一个概念的不同角度描述，DI是实现IoC的一种方法。

  

# 第二章：IoC容器

## 1、基于XML文件的配置

我们可以使用XML文件来描述IoC容器的配置信息。

```xml-dtd
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <bean id="a" class="com.java.service.A"></bean>
 
    <bean id="b" class="com.java.service.B"></bean>
 
    <bean id="work" class="com.java.service.Work">
        <property name="jiekou" ref="b"></property>
    </bean>
  
</beans>
```



### 1.1、创建Bean的三种方式

#### 1）基于构造方法

```xml-dtd
	<bean id="student" class="com.nanum.ioc.entity.Student">
        <property name="age" value="12"/>
        <property name="name" value="杰瑞"/>
    </bean>
```



#### 2）基于静态工厂方法（了解）

```xml-dtd
1、class指向静态工厂类 2、factory-method指向静态工厂方法
<bean id="student2"  class="com.nanum.ioc.entity.StudentFactory" factory-method="create">
        基于静态工厂方法创建的对象如果使用property还是会调用setter方法重新赋值
        <!--<property name="age" value="12"/>-->
        <!--<property name="name" value="杰瑞"/>-->
</bean>
```



#### 3）基于实例工厂方法





### 1.2、依赖注入

#### 1）通过setter/getter方法注入

```xml-dtd
<bean id="student" class="com.nanum.ioc.entity.Student">
	1、namet属性名	2、value中填写赋值内容，只能是基本数据类型及其包装类。
    <property name="age" value="12"/>
    <property name="name" value="杰瑞"/>
</bean>
```

#### 2）通过构造方法注入





## 2、基于Java类的配置

## 3、基于注解的配置
