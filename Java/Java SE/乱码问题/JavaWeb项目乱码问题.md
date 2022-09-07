# JavaWeb项目乱码问题

## 一、

tomcat配置：

- 在"service.xml"文件的Connector标签里加入”URIEncoding=UTF-8“

IntelliJ IDEA配置：

- 

![image-20210726173609268](C:\Users\null'pointer\AppData\Roaming\Typora\typora-user-images\image-20210726173609268.png)追加**-Dfile.encoding=UTF-8**；

![image-20210726173914145](C:\Users\null'pointer\AppData\Roaming\Typora\typora-user-images\image-20210726173914145.png)



## 二、Servlet乱码问题

示例图：

![](img/乱码示例.png)



解决方法：

调用`response`的`setContentType()`方法进行设置；

```java
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        //重点！！！！！！！！！！
        response.setContentType("text/html;charset=utf-8");

        System.out.println("你好，世界！");
        // Hello
        PrintWriter out = response.getWriter();
        out.println("<html><body>");
        out.println("<h1>" + message + "</h1>");
        out.println("</body></html>");
    }
```









## 三、Intellij IDEA控制台乱码问题

step01：打开"Edit Configurations“；

<img src="C:\Users\null'pointer\AppData\Roaming\Typora\typora-user-images\image-20210726171712990.png" alt="image-20210726171712990" style="zoom: 80%;" />

stet02：在VM options这一栏输入：`-Dfile.encoding=UTF-8`；

<img src="C:\Users\null'pointer\AppData\Roaming\Typora\typora-user-images\image-20210726172036568.png" alt="image-20210726172036568" style="zoom: 67%;" />

- setp03：重启服务器即可生效。