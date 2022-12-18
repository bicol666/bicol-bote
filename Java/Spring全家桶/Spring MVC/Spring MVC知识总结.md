# Spring MVC知识总结.md

#  简介

### 什么是Spring MVC？

Spring Web MVC 是基于 Servlet API 构建的原始 Web 框架，从一开始就包含在 Spring Framework 中。正式名称“Spring Web MVC”来自其源模块的名称（[`spring-webmvc`](https://github.com/spring-projects/spring-framework/tree/main/spring-webmvc)），但更常见的是“Spring MVC”。



### DisptcherServlet

Spring MVC 与许多其他 Web 框架一样，是围绕前端控制器模式设计的，其中一个中央控制器`Servlet`为`DispatcherServlet`请求处理提供共享算法，而实际工作由可配置的委托组件执行。该模型非常灵活，支持多种工作流程。

# 配置

### 坐标

- Maven

```xml
<!--servlet api-->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
    <scope>provided</scope>
</dependency>
<!--Spring MVC-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.20</version>
</dependency>
```

说明：Spring MVC是依赖Servlet构建的WEB框架，所以会依赖`servlet-api`。

# 简单示例

 



# 常用的注解

### @Controller

### @RequestMapping

衍生注解：PostMapping、GetMapping、PutMapping

### @RequestParam

### @RequestHeader



### @CookieValue

### @RequestAttribute

### @ModelAttribute

### @SessionAtttribute

### @SessionAttributes



# 核心技术

### 转发和重定向

#### 转发

- 需要配置好视图解析器

```xml
<!-- 视图解析器 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
    <!--前缀-->
    <property name="prefix" value="/WEB-INF/page/"/>
    <!--后缀-->
    <property name="suffix" value=".jsp"/>
</bean>
```

- 准备工作

在web资源目录下准备好WEB-INF/page/user.jsp。

```jsp
<%@ page contentType="text/html; charset=utf-8"
    pageEncoding="utf-8" %>
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>user</title>
    </head>
    <body>
        ${user}<br>
        ${user2}<br>
        ${user3}<br>
    </body>
</html>
```

- 转发

```java
@Controller
@RequestMapping("user")
public class UserController {
    // 转发
    // 1、【转发到视图】
    // 方式一、使用ModelAndView进行转发。
    // 这种方式只能转发到视图
    @RequestMapping("zf")
    public ModelAndView zf(ModelAndView modelAndView) {
        modelAndView.addObject("user", "张三、18岁");
        modelAndView.setViewName("user");
        return modelAndView;
    }


    // 方式二、直接返回视图文件名，
    // 视图解析器会拼接上配置好的前缀（/WEB-INF/page/）和后缀（.jsp）形成一个完整的资源路径(/WEB-INF/page/user.jsp)，
    // 同时可以使用Model来传递数据
    @RequestMapping("zf2")
    public String zf2(Model model) {
        // 在model里添加数据，
        model.addAttribute("user2", "李四，20岁");
        return "user";
    }



    // 2、【转发到请求方法】
    // 在前面添加前缀`forward:`，后面跟方法的（相对路径）URL。注意，是URL，不是资源文件名了！！！
    // 在同一个Controller下可以省略Controller的URL，例如在这里就可以省略user，直接写forward。
    @RequestMapping("zf3")
    public String zf3(Model model) {
        model.addAttribute("user3", "王五，22岁");
        return "forward:zf2";
    }


    // 3、【原生的Servlet里的转发方式】。
    // 这种方法不会走视图解析器，既可以转发到一个请求方法，也可以转发到一个视图。
    @RequestMapping("zf4")
    public void zf4(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setAttribute("user4", "赵六，24岁");
        // 转发发到视图
        //request.getRequestDispatcher("/WEB-INF/page/user.jsp").forward(request, response);
        // 转发到请求方法
        request.getRequestDispatcher("zf3").forward(request, response);
    }

}
```

#### 重定向

```java
// 重定向
// 方式一、添加前缀`redirect:`，后面跟要重定向的URL。
// 这里重定向到了`zf`，zf又转发给了视图，一共是两次请求。
@RequestMapping("/r")
public String redirect() {
    return "redirect:zf";
}

// 方式二、原生的Servlet里的重定向方式。
// 这里重定向到了`toUser2`，toUser2又转发给了toUser1处理，一共是两次请求。
@RequestMapping("/r2")
public void redirect2(HttpServletRequest request, HttpServletResponse response) throws IOException {
    response.sendRedirect("zf");
}
```



### 普通参数的获取

#### 从HTTP请求获取参数

- 【GET请求，从URL获取参数】

```java
// 使用注解@RequestParam，可以省略不写
@GetMapping("findById")
public String findById(@RequestParam Integer id) {
    return "id = " + id;
}
```

- 【POST请求，从请求体参数获取】

```java
// 使用注解@RequestParam，可以省略不写
// Content-Type只能是：application/x-www-urlencoded
@PostMapping("findById2")
public String findById2(@RequestParam Integer id) {
    return "id = " + id;
}
```

- 【从请求头参数获取】

```java
// 使用注解@RequestHeader,不可以省略
@PostMapping("findById3")
public String findById3(@RequestHeader Integer id) {
    return "id = " + id;
}
```

#### 从域对象获取参数

- 【从session域获取参数】

```java
// 先往session域里放数据（参数），然后转发到下面的请求方法
@GetMapping("test")
public void test(HttpSession session, HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    session.setAttribute("id", 666);
    request.getRequestDispatcher("findById4").forward(request, response);
}

// 使用注解@SessionAttribute获取参数
// session域对里没有参数会抛异常：ServletRequestBindingException
@GetMapping("findById4")
public String findById4(@SessionAttribute Integer id) {
    return "id = " + id;
}
```

- 【从request域获取参数】

```java
// 【从request域获取参数】
// 先往request域里放数据（参数），然后转发到下面的请求方法
@GetMapping("test2")
public void test2(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    request.setAttribute("id", 666);
    request.getRequestDispatcher("findById5").forward(request, response);
}

// 使用注解@RequestAttribute获取参数
@GetMapping("findById5")
public String findById5(@RequestAttribute Integer id) {
    return "id = " + id;
}
```



### JSON数据的获取与响应



### 配置URL与请求方式`@RequestMapping`

#### 配置URL

> 使用注解`@RequestMapping`及其衍生注解指定URL，一个请求方法可以指定多个URL，但是不同的请求方法不能指定同一个URL。

```java
@RequestMapping("zf")
public ModelAndView zf(ModelAndView modelAndView) {
    modelAndView.addObject("user", "张三、18岁");
    modelAndView.setViewName("user");
    return modelAndView;
}
```

#### 配置请求方式

1、可以通过method指定请求方式

```java
@RequestMapping(value = "findById",method = RequestMethod.GET)
```

2、也可以使用`@RequestMapping`的衍生注解，其衍生注解就相当于指定了请求方式（method）的`@RequestMapping`。

```java
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@RequestMapping(
    method = {RequestMethod.GET}
)
public @interface GetMapping {}
```



### 字符集

### 数据转化

### 全局异常处理

#### 1、在Controller中使用注解：`@ExceptionHandler`

> 在**Controller中**创建一个使用了`@ExceptionHandler`注解的方法来处理异常。

💡**该方法需满足以下条件：**

- 访问权限必须为`public`。
- 返回值与普通“controller”一样。
- 形参列表必须包含至少一个异常类型，例如：Exception、Throwable。表示将会捕捉到的异常。
- 添加注解：`@ExceptionHandler`（在**SSM**环境下请添加`<mvc:annotation-driven />`开启注解扫描）。

📃**示例**

```java
//定义Exception解决未被controller处理的异常
@ExceptionHandler(Exception.class)
@ResponseStatus(HttpStatus.OK)
@ResponseBody
public Object handleException(HttpServletRequest request, Exception e) {
    e.printStackTrace();
    Map<String, Object> errorInfo = new HashMap<>();
    if (e instanceof BusinessException) {
        BusinessException businessException = (BusinessException) e;
        errorInfo.put("errorCode", businessException.getErrorCode());
        errorInfo.put("errorMsg", businessException.getErrorMsg());
    } else {
        errorInfo.put("errorCode", EMBusinessError.UNKNOWN_ERROR.getErrorCode());
        errorInfo.put("errorMsg", EMBusinessError.UNKNOWN_ERROR.getErrorMsg());
    }
    return CommonResponse.create(Constant.HANDLE_STATUS_FAIL, errorInfo);
}
```

🚨**警告**

这种方式只能处理**当前Controller**产生的异常（包括当前Controller调用的Service、Dao层产生的异常）。为了对异常做统一处理，推荐的做法是定义一个基类（BaseController），把异常处理的方法定义在基类里，所有`Controller`继承这个基类。



#### 2、实现接口：HandlerExceptionResolver【不推荐】

> 实现`HandlerExceptionResolver`接口来自定义异常解析处理器，然后将其注入`IoC`容器即可。

**📕不推荐的原因：**

使用这种方式来处理异常会导致`Spring MVC`默认的三种异常处理器失效，会导致`@ExceptionHandler`不可用。因为`DisptcherServlet`在初始化的时候发现`IoC`容器中已有我们注入的`HandlerExceptionResolver`类型的Bean就不会使用默认的Bean。



#### 3、使用注解`@ControllerAdvice`

> 创建一个使用`@ControllerAdvice`标识的类来处理异常，类中定义不同的方法来处理不同的异常，方法需使用注解`ExceptionHandler`指定要捕获的异常。
>
> @RestControllerAdvice是`@ControllerAdvice`的衍生注解，使用后异常处理结果不走视图解析器，直接返回。

`GlobalExceptionHadler.java`

```java
@RestControllerAdvice // 采取@RestControllerAdvice：异常处理结果不走视图解析器，直接返回。
public class GlobalExceptionHandler {

    // 使用 @ExceptionHandler指定要捕获的异常
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



### 静态资源放行



### 拦截器



### 编程式配置



### 跨域解决方案



### Restful风格API



### 文件上传与下载



### WebSocket



### 整合MyBatis