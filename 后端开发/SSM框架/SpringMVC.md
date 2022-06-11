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



---

​	

# 3、HelloSpringMVC程序

1. 新建一个Maven项目，**添加Web支持**；

2. 导入**SpringMVC依赖**；

   ```xml
   <dependencies>
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>5.1.9.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>servlet-api</artifactId>
               <version>2.5</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet.jsp</groupId>
               <artifactId>jsp-api</artifactId>
               <version>2.2</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>jstl</artifactId>
               <version>1.2</version>
           </dependency>
       </dependencies>
   ```

3. 在**web.xml**配置文件中， **注册DispatcherServlet**；

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
            version="5.0">
   
       <!--1.注册DispatcherServlet-->
       <servlet>
           <servlet-name>springmvc</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--关联一个springmvc的配置文件:【servlet-name】-servlet.xml-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:springmvc-servlet.xml</param-value>
           </init-param>
           <!--启动级别-1-->
           <load-on-startup>1</load-on-startup>
       </servlet>
   
       <!--/ 匹配所有的请求；（不包括.jsp）-->
       <!--/* 匹配所有的请求；（包括.jsp）-->
       <servlet-mapping>
           <servlet-name>springmvc</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   
   </web-app>
   ```

4. 编写**SpringMVC 的 配置文件**【springmvc-servlet.xml】，这里的名称要求是按照官方来的；

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--添加 处理映射器-->
       <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
       <!--添加 处理器适配器-->
       <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
       <!--添加 视图解析器 DispatcherServlet提供的ModelAndView-->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
           <!--前缀-->
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <!--后缀-->
           <property name="suffix" value=".jsp"/>
       </bean>
   
       <!--将类交给SpringIOC容器，注册bean-->
       <bean id="/hello" class="com.xleixz.controller.HelloController"/>
   </beans>
   ```

   > 处理映射器

   ```xml
   <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
   ```

   > 处理适配器

   ```xml
   <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
   ```

   > 视图解析器

   ```xml
   <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
         <!--前缀-->
         <property name="prefix" value="/WEB-INF/jsp/"/>
         <!--后缀-->
         <property name="suffix" value=".jsp"/>
   </bean>
   ```

5. 编写我们要操作业务Controller ，要么实现Controller接口，要么增加注解；需要返回一个ModelAndView，装数据，封视图；

   ```java
   import org.springframework.web.servlet.ModelAndView;
   import org.springframework.web.servlet.mvc.Controller;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   
   //注意：这里我们先导入Controller接口
   public class HelloController implements Controller {
   
       @Override
       public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
           //ModelAndView 模型和视图
           ModelAndView mv = new ModelAndView();
   
           //封装对象，放在ModelAndView中。Model
           mv.addObject("msg", "HelloSpringMVC!");
           //封装要跳转的视图，放在ModelAndView中
           mv.setViewName("hello"); //: /WEB-INF/jsp/hello.jsp
           return mv;
       }
   }
   ```

6. 将类交给SpringIOC容器【springmvc-servlet.xml】，注册bean；

   ```xml
   <!--Handler-->
   <bean id="/hello" class="com.kuang.controller.HelloController"/>
   ```

7. 写要跳转的jsp页面，显示ModelandView存放的数据，以及我们的正常页面；

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Hello</title>
   </head>
   <body>
   测试成功！
   </body>
   </html>
   ```

​		

<font color = "red">**可能遇到的问题：访问出现404，排查步骤：**</font>

1. 查看控制台输出，看一下是不是缺少了什么jar包。

2. 如果jar包存在，显示无法输出，就在IDEA的项目发布中，添加lib依赖！

   - 点击FIle - Project Structure - Artifacts - 选择对应的项目 - 在**WEB-INF文件夹**下新建**lib** - 点击添加**Library Files** - 将**除了（servlet，jstl，jsp）其他的所有jar包**导入 - OK

     ![image-20220611112808532](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220611112808532.png)

3. 重启Tomcat 即可解决！

<font color = "green">**原因：**</font>Maven无法扫描Webapp下的jar包！





