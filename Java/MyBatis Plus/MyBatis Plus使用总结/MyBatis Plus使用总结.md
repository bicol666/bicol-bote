# MyBatis Plus使用总结.md

# CRUD操作

### 插入一条记录

```java
int insert(T entity)
```

#### **1、参数说明**

| 参数类型 | 参数名 | 描述               |
| -------- | ------ | ------------------ |
| T        | entity | 要插入的实体对象。 |



#### **2、获取自增的ID值**

insert()方法会将自动生成的**ID**回填到要插入的实体对象的ID字段中。



**注意**

要插入的实体对象的ID字段如果未指定`@TableId`注解，默认会是`@TableId(type = IdType.NONE)`，也就是使用MyBatis Plus生成的ID，而不是数据库生成的ID。这个由MP生成的ID会在实体对象被插入到数据表之前赋值给实体对象。

想要使用数据库生成的ID，应该在实体类的ID属性添加`@TableId(type = IdType.AUTO)`注解。



#### **3、实体对象属性名与数据表字段名不一致（非驼峰）**

```java
通过value指定数据表字段名。
@TableField(value = "username")
private String name;
```



#### **4、实体对象属性在数据表中不存在**

```java
说明改属性在表中不存在
@TableField(exist = false)
private String phone;
```



### 根据ID更新记录

```java
int updateById(@Param(Constants.ENTITY) T entity);
```

**参数说明**

| 参数类型 | 参数名 | 描述               |
| -------- | ------ | ------------------ |
| T        | entity | 要更新的实体对象。 |



### 根据条件更新记录

```java
int update(@Param(Constants.ENTITY) T updateEntity,
           @Param(Constants.WRAPPER) Wrapper<T> whereWrapper);
```

**参数说明**

| 参数类型   | 参数名        | 描述                                                         |
| ---------- | ------------- | ------------------------------------------------------------ |
| T          | updateEntity  | 要更新的实体对象。                                           |
| Wrapper<T> | updateWrapper | 实体对象封装操作类（可以为 null,里面的 entity 用于生成 where 语句） |

