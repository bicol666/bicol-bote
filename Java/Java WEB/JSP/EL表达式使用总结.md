# 一、EL(Expression Language)表达式简介

> EL表达式可以在JSP页面中非常简便地获取数据，但是不能设置数据；
>
> 用于代替JSP表达式脚本；
>
> **语法格式：**`${expression}`



# 二、从四大域中获取数据

## 1、未指定EL内置域对象

> EL可以从四大域中获取属性，格式为：`${属性名}`
>
> 如果没有指定内置域对象，会根据内置域对象的范围大小，从小到大查找属性名对应的属性值，找到之后就不再继续往下找；

**示例：**

```jsp
   <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
        <html>
        <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
        <title>Insert title here</title>
        </head>
        <body>
            <%
                pageContext.setAttribute("name", "linjie");
                request.setAttribute("name", "lucy");
                session.setAttribute("name", "king");
                application.setAttribute("name", "bilibili");
            %>
           
            <%-- 
    		由于未指定到哪个对象(域)中获取属性，会根据内置域对象的范围大小，从小到大查找属性名对应的属性值；
            由于范围最小的域是pageCOntext，所以表达式的结果为：linjie； --%>
            name=${name }
        </body>
        </html>
```

## 2、指定EL内置域对象，从指定域中获取数据，提高查找效率



**格式：**`${EL内置域对象.属性名}`

**EL四大内置域对象：**

- pageScope：获取pageContext域中的数据；
- requestScope：获取ServletRequest域中的数据；
- sessionScope：获取Session域中的数据；
- applicationScope：获取ServletContext域中的数据；

**示例：**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
    <%
        pageContext.setAttribute("name", "linjie");
        request.setAttribute("name", "lucy");
        session.setAttribute("name", "king");
        application.setAttribute("name", "bilibili");
    %>
    name=${applicationScope.name }
</body>
</html>
```

# 三、EL11个内置对象

| 变量             | 类型                 | 作用                                            |
| ---------------- | -------------------- | ----------------------------------------------- |
| pageContext      | PageContextImpl      | 获取JSP中的九大内置对象；                       |
| pageScope        | Map<String,Object>   | 获取pageContext域中的数据；                     |
| sessionScope     | Map<String,Object>   | 获取session域中的数据；                         |
| applicationScope | Map<String,Object>   | 获取ServletContext域中的数据；                  |
| requestScope     | Map<String,Object>   | 获取ServletRequest域中的数据；                  |
| param            | Map<String,String>   | 获取请求参数；                                  |
| paramValues      | Map<String,String[]> | 获取有多个值的请求参数；                        |
| cookie           | Map<String,Cookie>   | 获取当前请求的Cookie；                          |
| initParam        | Map<Stirng,String>   | 获取web.xml中的`<context-params>`的上下文参数； |
|                  |                      |                                                 |

# 四、EL运算

## 1、关系运算



## 2、逻辑运算



## 3、位运算



## 4、点.运算&中括号[]运算
