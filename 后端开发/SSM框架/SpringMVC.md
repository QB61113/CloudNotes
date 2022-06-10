# SpringMVC

# 1、MVC介绍

## 1.1 什么是MVC

- MVC是模型(`Model` - dao、service层)、视图(`View` - 数据 - jsp)、控制器(`Controller` - Servlet)的简写，是一种软件设计规范。
- 是将业务逻辑、数据、显示分离的方法来组织代码。
- MVC主要作用是**降低了视图与业务逻辑间的双向偶合**。
- MVC不是一种设计模式，**MVC是一种架构模式**。当然不同的MVC存在差异。

​	

**Model（模型）：**数据模型，提供要展示的数据，因此包含数据和行为，可以认为是领域模型或JavaBean组件（包

含数据和行为），不过现在一般都分离开来：Va lue Object（数据Dao） 和 服务层（行为Service）。也就是模型提

供了模型数据查询和模型数据的状态更新等功能，包括数据和业务。

**View（视图）：**负责进行模型的展示，一般就是我们见到的用户界面，客户想看到的东西。

**Controller（控制器）：**接收用户请求，委托给模型进行处理（状态改变），处理完毕后把返回的模型数据返回给

视图，由视图负责展示。也就是说控制器做了个调度员的工作。

**最典型的MVC就是JSP + servlet + javabean的模式。**

​	

前端 	数据传输	实体类

实体类：用户名，密码，生日，爱好……

前端：用户名，密码

​	

pojo：User

vo：UserVo

在pojo中分出一个vo实体类给前端使用，这样可以避免实体类中很多前端用不到的属性，避免了属性的空值

​	

> **Model1时代**

早期开发中使用的是Model1时代，主要分为两层，视图层和模型层。在当前页面将数据返回到页面。

**JSP：本质就是一个Servlet**

> **Model2时代**

Model2把一个项目分成三部分，包括**视图、控制、模型。**

​	

面试题：你的项目架构，是设计好的，还是演进的？

回答：演进的

​	

**职责分析**

**Controller：控制器**

1. 获取表单数据
2. 调用业务逻辑
3. 转向指定的页面

就是Servlet！

**Model：模型**

1. 业务逻辑
2. 保存数据的状态

相当于service层和dao层

**View：视图**

1. 显示页面

相当于jsp

​	

## 1.2 回顾Servlet

**MVC框架要做哪些事情**

1. 将url映射到java类或java类的方法。
2. 封装用户提交的数据。
3. 处理请求--调用相关的业务处理--封装响应数据。
4. 将响应的数据进行渲染 . jsp / html 等表示层数据。

**说明：**

常见的服务器端MVC框架有：Struts、Spring MVC、ASP.NET MVC、Zend Framework、JSF；常见前端MVC框架：

vue、angularjs、react、backbone；由MVC演化出了另外一些模式如：MVP、MVVM 等等....

---

​	

# 2、SpringMVC介绍

**Spring MVC是Spring Framework的一部分，是基于Java实现MVC的轻量级Web框架。**

[SpringMVC官网文档](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html "SPringMVC官网文档")

 Spring MVC的特点：

1. 轻量级，简单易学
2. 高效 , 基于请求响应的MVC框架
3. 与Spring兼容性好，无缝结合
4. 约定优于配置（约定好的东西不要去改）
5. 功能强大：RESTful、数据验证、格式化、本地化、主题等
6. 简洁灵活

​	

> Spring的web框架围绕**DispatcherServlet** [ 调度Servlet ] 设计。
>
> DispatcherServlet的作用是将请求分发到不同的处理器。从Spring 2.5开始，使用Java 5或者以上版本的用户可
>
> 以采用基于注解形式进行开发，十分简洁；正因为SpringMVC好 , 简单 , 便捷 , 易学 , 天生和Spring无缝集成(使
>
> 用SpringIoC和Aop) , 使用约定优于配置 . 能够进行简单的junit测试 . 支持Restful风格 .异常处理 , 本地化 , 国际
>
> 化 , 数据验证 , 类型转换 , 拦截器 等等......所以我们要学习。

**最重要的一点还是用的人多 , 使用的公司多。**

​	

## 2.1 中央控制器

Spring的web框架围绕**DispatcherServlet**设计。Dispatcher（调度员）Servlet的作用是将请求分发到不同的处理器。

![image-20220610185630069](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220610185630069.png)

从Spring 2.5开始，使用Java 5或者以上版本的用户可以采用基于注解的controller声明方式。

Spring MVC框架像许多其他MVC框架一样, **以请求为驱动** , **围绕一个中心Servlet分派请求及提供其他功能，**

**DispatcherServlet是一个实际的Servlet (它继承自HttpServlet 基类)**。

**SpringMVC的原理如下图所示：**

​	当发起请求时被前置的控制器拦截到请求，根据请求参数生成代理请求，找到请求对应的实际控制器，控制器处

理请求，创建数据模型，访问数据库，将模型响应给中心控制器，控制器使用模型与视图渲染视图结果，将结果返

回给中心控制器，再将结果返回给请求者。

![image-20220610190520975](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220610190520975.png)

​	

## 2.2 SpringMVC执行原理

