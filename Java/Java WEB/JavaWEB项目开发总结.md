# JavaWEB项目开发总结

# 一、项目的开发流程

**架构设计：**

- 物理架构设计：

  应用服务器的选取：tomcat、weblogic、websphere、jboss……

  数据库服务器的选取：mysql、oracle、sqlserver、达梦（国产）……

- 逻辑架构设计：代码分层

  视图层---》控制层---》业务层---》持久层

- 技术选型：采取什么技术开发项目

**项目设计：**

- 物理模型设计（数据库表的设计）：哪些表、哪些字段、字段的类型长度、表与表之间的关系……

  物理建模工具：PowerDesigner

- 逻辑模型设计：那些类、哪些属性和方法、方法的参数和返回值、类和类之间的关系……

  RationalRose

- 界面设计

  企业级应用）界面朴素   互联网应用）界面炫酷好看

- 算法设计：企业级应用不太涉及

**搭建开发环境：**

**编码开发、测试**

# 二、技术架构

JavaWEB开发的四层技术架构：

- **视图层**（View）：1. 展示、采集数据 2. 与用户交互；

  - 前端

    原生技术：`html`、`css`、`JavaScript`	

    框架：`bootstrap`、`jquery`、`vue`、`react`……

  - 后端

    原生技术：`jsp（淘汰）`

    框架（模板引擎）：`thymeleaf`……

- **控制层**（Controller）：控制整个WEB请求流程：获取参数，调用不同的Service处理请求，响应结果；

  - 原生技术：`sevlet`
  - 框架：`SpringMVC`、`struts（淘汰）`……

- **业务层**（Service）：处理具体的业务逻辑；

  - 原生技术：`JavaSE`
  - 框架（工作流）：`Activity`……

- **持久层**（Dao/Mapper）：对数据库表的CRUD；

  - 原生技术：`JDBC`
  - 框架：`MyBatis`、`Hibernate（淘汰）`……

- （整合层）：每一层都可以用；

  `Spring（IOC、AOP）`、`SpringBoot`



