# Hibernate Validator使用总结.md

# 相关概念

- **JSR**: Java规范提案（Java Specification Requests）,是指向JCP（Java Community Process）提出新增一个标准化技术规范的正式请求。任何人都可以提交JSR，以向Java平台增添新的API和服务。JSR已成为Java界的一个重要标准。

- **JSR-303**：JSR-303 是JAVA EE 6 中的一项子规范，叫做Bean Validation。
- **Hibernate Validator**：`Hibernate Validator` 是 Bean Validation 的参考实现 。`Hibernate Validator` 提供了 JSR 303 规范中所有内置 constraint （约束）的实现，除此之外还有一些附加的 constraint。



`约束规则注解`

| 注解                                         | 适用字段                                               | 注解说明                                                     | 注意事项                                                     |
| -------------------------------------------- | ------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| @NotNull / @Null                             | 引用数据类型                                           | 验证值是否为非空 / 空                                        |                                                              |
| @AssertTrue / @AssertFalse                   | boolean                                                | 验证值是否为true / false                                     |                                                              |
| @Max / @Min                                  | BigDecimal、BigInteger、String、byte、short、int、long | 验证值是否小于或者等于指定的整数值                           | 建议使用在Stirng,Integer类型，不建议使用在int类型上，因为表单提交的值为“”时无法转换为int |
| @DecimalMax / @DecimalMin                    | BigDecimal、BigInteger、String、byte、short、int、long | 验证值是否小于或者等于指定的小数值，要注意小数存在精度问题。 |                                                              |
| @Future / @Past                              | Date、Calendar                                         | 验证值是否在当前时间之后 / 之前。                            |                                                              |
| @NotBlank/@NotEmpty                          | 引用数据类型                                           | @NotEmpty：检查约束元素是否为Null或者是EMPTY。@NotBlank：检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格。 | 区别：空格（" "）对于 NotEmpty 是合法的，而 NotBlank 会抛出校验异常 |
| @Pattern                                     | String                                                 | 验证值是否匹配正则表达式                                     | regexp:正则表达式flags: 指定Pattern.Flag 的数组，表示正则表达式的相关选项。 |
| @Size                                        | String、Collection、Map、数组                          | 验证值是否满足长度要求，max:指定最大长度，min:指定最小长度。 |                                                              |
| @Length(min=, max=)                          | String                                                 | 专门应用于String类型，验证字符串是否满足长度要求。           |                                                              |
| @Range(min=, max=)                           |                                                        | 被指定的元素必须在合适的范围内                               |                                                              |
| @CreditCardNumber                            |                                                        | 信用卡验证                                                   |                                                              |
| @Email                                       |                                                        | 验证是否是邮件地址                                           | 如果为null,不进行验证，算通过验证。                          |
| @URL(protocol=,host=, port=,regexp=, flags=) |                                                        | 验证URL地址                                                  |                                                              |
| @Valid                                       | 递归的对关联对象进行校验                               | 如果关联对象是个集合或者数组,那么对其中的元素进行递归校验,如果是一个map,则对其中的值部分进行校验(是否进行递归验证) |                                                              |



# 基本配置

### 坐标

- maven坐标

```xml
<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
    <version>2.0.1.Final</version>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>6.0.9.Final</version>
</dependency>
```

说明：``hibernate-validator`是传递依赖`validation-api`的，所以可以不用添加`validation-api`的坐标。



### Spring MVC XML配置

```xml
<bean id="localValidator" class="org.springframework.validation.beanvalidation.LocalValidatorFactoryBean">
    <property name="providerClass" value="org.hibernate.validator.HibernateValidator"/>
</bean>
<!--注册注解驱动-->
<mvc:annotation-driven validator="localValidator"/>
```



### SpringBoot配置



# 简单使用



# 在SpringMVC中的使用

### 基本使用

- 在`pojo类`的属性上加具体的校验规则注解，message里写提示信息。
- 使用`@Validated`标识要校验的对象，使用`BindingResult`或者`Errors`来接收校验结果。

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    
    @NotBlank(message = "用户名不能为空")
    private String username;

    @Max(value = 150,message = "年龄最大150岁")
    private Integer age;
    
}
```



```java
// 方式一、使用`@Validated`来标识要校验的pojo类，使用`BindingResult`来接收校验结果。
@PostMapping("insert1")
public String insert1(@Validated @RequestBody User user, BindingResult result) {
    List<ObjectError> allErrors = result.getAllErrors();
    allErrors.forEach(i -> {
        System.out.println(i.getDefaultMessage());
    });
    System.out.println("插入用户：" + user);
    return "success";
}

// 方式二、使用`@Validated`来标识要校验的pojo类，使用`Errors`来接收校验结果。
@PostMapping("insert2")
public String insert2(@Validated @RequestBody User user, Errors errors) {
    List<ObjectError> allErrors = errors.getAllErrors();
    allErrors.forEach(i -> {
        System.out.println(i.getDefaultMessage());
    });
    System.out.println("插入用户：" + user);
    return "success";
}

// 方式三、不使用`BindingResult`或`Errors`来接收校验结果，校验不合格就会抛出异常：MethodArgumentNotValidException
@PostMapping("insert3")
public String insert(@Validated @RequestBody User user) {
    System.out.println("插入用户：" + user);
    return "success";
}
```



### 搭配统一异常处理

> 采取方式三，不使用`BindingResult`或`Errors`来接收校验结果，当检验不合格时抛出异常，捕获异常进行统一处理。

```java
@RestControllerAdvice // 采取@RestControllerAdvice：异常处理结果不走视图解析器，直接返回。
public class GlobalExceptionHandler {

    // 校验不合格产生异常（MethodArgumentNotValidException）时在这里进行统一处理
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public Map<String, Object> validExceptionHandler(MethodArgumentNotValidException e) {
        // 获取所有校验不合格的Error
        List<FieldError> fieldErrors = e.getBindingResult().getFieldErrors();
        // 封装成一个Map，key为不合格字段名，value为提示信息message。
        Map<String, Object> errorMap = fieldErrors.stream()
            .collect(Collectors.toMap(item -> item.getField(), item -> item.getDefaultMessage()));
        return errorMap;
    }

}
```

`返回示例`

```json
{
    "age": "年龄最大150岁",
    "username": "用户名不能为空"
}
```



  

# 参考文献

- [Java中的参数校验](https://blog.csdn.net/JewaveOxford/article/details/107701871)

- [java中优雅的参数校验方法](https://blog.csdn.net/WX5991/article/details/120881927)