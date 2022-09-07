# 一、依赖地址

Gradle坐标：

```groovy
implementation 'org.mybatis:mybatis:3.5.7'
```

Maven坐标：

```xml
```



# 二、Mybatis之配置文件

### 1）**MyBatisConfig.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--在这里指定dtd文档约束文件的url，集成开发环境会根据url去获取-->
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--配置Mybatis中数据库连接环境，可以配置多个，default属性指定当前使用的是哪个环境-->
    <environments default="mysql">
        
        <environment id="mysql">
            <!--配置Mybatis当中的事务 和JDBC一致-->
            <transactionManager type="JDBC"></transactionManager>
            
            <!--配置数据库连接池-->
            <dataSource type="POOLED">
				<!--JDBC连接四大参数配置-->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url"value="jdbc:mysql://127.0.0.1:3306/mybatis_test?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf8&amp;serverTimezone=UTC"/>
                <property name="username" value="root"/>
                <property name="password" value="248650"/>
            </dataSource>
        </environment>

    </environments>

    <!--注册要加载的Mapper映射文件-->
    <mappers>
        <mapper resource="UserMapper.xml"/>
    </mappers>
</configuration>
```

### 2）读取属性文件

---

mybatis支持对属性文件的读取，也就是说datasource元素里面`property元素`的值可以用`properties文件`中的内容代替，以达到解耦合的目的；

- 这种方式：

```xml
            <dataSource type="POOLED">
				<!--JDBC连接四大参数配置-->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url"value="jdbc:mysql://127.0.0.1:3306/mybatis_test?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf8&amp;serverTimezone=UTC"/>
                <property name="username" value="root"/>
                <property name="password" value="248650"/>
            </dataSource>
```



- 可以替换为以下这种方式：

mybaitis配置文件：**MyBatisConfig.xml**

```xml
<properties resource="db.properties"></properties>         
<dataSource type="POOLED">
                <property name="driver" value="${drivername}"/>
                <property name="url"value="${name}"/>
                <property name="username" value="${password}"/>
                <property name="password" value="${url}"/>
</dataSource>
```

属性文件：**db.properties**

```properties
drivername=com.mysql.cj.jdbc.Driver
name=用户名
password=密码
url=jdbc:mysql://127.0.0.1:3306/数据库?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf8&amp;serverTimezone=UTC
```

# 三、Mybatis之映射文件

## 1、select查询的三种方式

### 1）查询单个对象

映射文件：UserMapper.xml

```xml
<mapper namespace="UserMapper"> 
    <select id="selectOne" resultType="user">
        select * from user where uid = 1
    </select>
</mapper>
```



```java
String resource = "mybatis.xml";
InputStream asStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(asStream);
SqlSession sqlSession = sqlSessionFactory.openSession();

//调用selectOne()方法返回单个对象；
User user = sqlSession.selectOne("UserMapper.selectOne");
System.out.println("user = " + user);
```



### 2）查询多个对象，返回List集合

映射文件：UserMapper.xml

```xml
<mapper namespace="UserMapper">
    <select id="selectAllUser" resultType="user">
        select * from user
    </select>
</mapper>
```



```java
String resource = "mybatis.xml";
InputStream asStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(asStream);
SqlSession sqlSession = sqlSessionFactory.openSession();

//调用selectList()方法返回查询到的对象的List集合；
List<User> users = sqlSession.selectList("UserMapper.selectAllUser");
users.forEach((user)->{
    System.out.println(user);
});
```



### 3）查询多个对象，返回Map集合

映射文件：UserMapper.xml

```xml
<mapper namespace="UserMapper">
    <!--注意：这里的返回类型是map-->
    <select id="selectAllUser" resultType="map">
        select * from user
    </select>
</mapper>
```



```java
String resource = "mybatis.xml";
InputStream asStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(asStream);
SqlSession sqlSession = sqlSessionFactory.openSession();

//注意：selectMap()方法第二个参数是选择将那哪一字段当做Map的key；
Map<String,User> users = sqlSession.selectMap("UserMapper.selectAllUser","uid");
System.out.println("users = " + users);
```

## 2、接收参数的方式

### 1）参数是：基本数据类型以及String类型

> 这类参数使用`#{param1}`去接收；
>
> 1、当只有一个参数(基本数据类型以及String类型)时，`#{param1}`里的参数名可以随意；
>
> 2、当有多个参数(基本数据类型以及String类型)时：
>
> ​	方式一：`#{param1}`、`#{param2}`、`#{param3}`、`#{param……}`
>
> ​	方式二：`#{arg0}`、`#{arg1}`、`#{arg2}`、`#{arg……}`
>
> ps:注意两种方式的起始下标顺序；

```java
//第二个参数就是传给下面的sql语句的参数；
UsersqlSession.selectOne("UserMapper.selectAllUser",12);
```



```xml
<mapper namespace="UserMapper">
    <!--注意：这里的返回类型是map-->
    <select id="selectAllUser" resultType="user">
        select * from user where id = #{param1}
    </select>
</mapper>
```

### 2）参数是：普通(自定义)对象

> 这类参数使用`#{对象的属性}`即可接收；



### 3）参数是：集合(array、list、map)类型

> 接收这类参数需要使用<foreach>标签去接收；
>
> 使用<foreach>标签可以遍历集合中的每一个元素，动态生成sql语句；

