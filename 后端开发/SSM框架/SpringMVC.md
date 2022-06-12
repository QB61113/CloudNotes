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

**简要分析执行流程**

![image-20220611145922253](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220611145922253.png)

1. DispatcherServlet表示前置控制器，是整个SpringMVC的控制中心。用户发出请求，DispatcherServlet接收请求

   并拦截请求。

   - 我们假设请求的url为 : http://localhost:8080/SpringMVC/hello

   - **如上url拆分成三部分：**

   - http://localhost:8080服务器域名

   - SpringMVC部署在服务器上的web站点

   - hello表示控制器

   - 通过分析，如上url表示为：请求位于服务器localhost:8080上的SpringMVC站点的hello控制器。

2. HandlerMapping为处理器映射。DispatcherServlet调用HandlerMapping,HandlerMapping根据请求url查找

   Handler。

3. HandlerExecution表示具体的Handler,其主要作用是根据url查找控制器，如上url被查找控制器为：hello。

4. HandlerExecution将解析后的信息传递给DispatcherServlet,如解析控制器映射等。

5. HandlerAdapter表示处理器适配器，其按照特定的规则去执行Handler。

6. Handler让具体的Controller执行。

7. Controller将具体的执行信息返回给HandlerAdapter,如ModelAndView。

8. HandlerAdapter将视图逻辑名或模型传递给DispatcherServlet。

9. DispatcherServlet调用视图解析器(ViewResolver)来解析HandlerAdapter传递的逻辑视图名。

10. 视图解析器将解析的逻辑视图名传给DispatcherServlet。

11. DispatcherServlet根据视图解析器解析的视图结果，调用具体的视图。

12. 最终视图呈现给用户。

---

​	

# 3、HelloSpringMVC程序（配置版）

> 配置版SpringMVC较为繁琐，**注解版**才是SpringMVC的精髓。

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

3. 在**web.xml**配置文件中， **注册DispatcherServlet**；**这是一个SpringMVC的核心: 请求分发器，前端控制器**；

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
            version="5.0">
   
      <!--1.注册配置DispatcherServlet  这是一个SpringMVC的核心: 请求分发器，前端控制器-->
       <servlet>
           <servlet-name>springmvc</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--绑定一个springmvc的配置文件:【servlet-name】-servlet.xml-->
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

4. 编写**SpringMVC 的 配置文件**【springmvc-servlet.xml】，这里的名称要求尽量按照官方来【(servletname)-servlet.xml】；

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--添加 处理器映射器-->
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

   > 处理器映射器

   ```xml
   <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
   ```

   > 处理器适配器

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

---

​	

# 4、注解开发SpringMVC

1. 新建一个Maven项目，**添加Web支持**，导入依赖；<font color="red">（**记住配置静态资源过滤**）、（**记住要在Project Structure中的Artifacts中导入除了3个Servlet包以外的其他jar包 **）</font>

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
   
   
       <build>
           <resources>
               <resource>
                   <directory>src/main/java</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                   </includes>
                   <filtering>false</filtering>
               </resource>
               <resource>
                   <directory>src/main/resources</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                   </includes>
                   <filtering>false</filtering>
               </resource>
           </resources>
       </build>
   ```

2. 配置**web.xml**文件，**注册DispatchServlet前置控制器**

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
            version="5.0">
   
       <!--1.注册DispatchServlet-->
       <servlet>
           <servlet-name>SpringMVC</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:springmvc-servlet.xml</param-value>
           </init-param>
           <!-- 启动顺序，数字越小，启动越早 -->
           <load-on-startup>1</load-on-startup>
       </servlet>
   
       <!--所有请求都会被springmvc拦截 -->
       <servlet-mapping>
           <servlet-name>SpringMVC</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   
   </web-app>
   ```

3. **添加Spring MVC配置文件**【springmvc-servlet.xml】

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd
          http://www.springframework.org/schema/mvc
          https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
       <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
       <context:component-scan base-package="com.xleixz.controller"/>
       <!-- 让Spring MVC不处理静态资源 .css  .html  .js -->
       <mvc:default-servlet-handler />
       <!--
       支持mvc注解驱动
           在spring中一般采用@RequestMapping注解来完成映射关系
           要想使@RequestMapping注解生效
           必须向上下文中注册DefaultAnnotationHandlerMapping
           和一个AnnotationMethodHandlerAdapter实例
           这两个实例分别在类级别和方法级别处理。
           而annotation-driven配置帮助我们自动完成上述两个实例的注入。
        -->
       <mvc:annotation-driven />
   
       <!-- 视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
             id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/" />
           <!-- 后缀 -->
           <property name="suffix" value=".jsp" />
       </bean>
   
   </beans>
   ```

   > 自动扫描包

   ```xml
   <context:component-scan base-package="com.xleixz.controller"/>
   ```

   > 过滤静态资源，让Spring MVC不处理静态资源，例如：.css  .html  .js

   ```xml
   <mvc:default-servlet-handler />
   ```

   > 注册DefaultAnnotationHandlerMapping（处理器映射器），和一个AnnotationMethodHandlerAdapter（处理器适配器）实例

   ```xml
   <mvc:annotation-driven />
   ```

   > 视图解析器

   ```xml
   <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
             id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/" />
           <!-- 后缀 -->
           <property name="suffix" value=".jsp" />
       </bean>
   ```

4. **创建Controller业务层**

   ```java
   import org.springframework.stereotype.Controller;
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.RequestMapping;
   
   //实现了Controller注解，表示这是一个控制器
   @Controller
   public class HelloController {
       //实现了RequestMapping注解，表示这是一个请求映射，这是一个映射地址
       @RequestMapping("/hello")
       public String hello(Model model) {
           //封装数据
           model.addAttribute("hello", "你好");
           return "hello";//会被视图解析器解析处理到hello.jsp
       }
   }
   ```

   > 实现Controller注解，表示一个控制器

   ```xml
   @Controller
   ```

   > 实现RequestMapping注解，表示这是一个请求映射，这是一个映射地址

   ```xml
   @RequestMapping("")
   ```

   > 方法中声明Model类型的参数是为了把Action中的数据带到视图中

   ```xml
   model.addAttribute("hello", "你好");
   ```

   > 方法返回的结果是视图的名称hello，加上配置文件中的前后缀变成/**hello**.jsp

   ```xml
   return "hello"
   ```

5. 创建**视图层**【hello.jsp】

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <hr>
   <h1>测试成功！</h1>
   </hr>
   </body>
   </html>
   ```

​	

使用springMVC必须配置的三大件：

<font color="green">**处理器映射器、处理器适配器、视图解析器**</font>

通常，只需要**手动配置视图解析器**，而**处理器映射器**和**处理器适配器**只需要开启**注解驱动**即可，而省去了大

段的xml配置。

---

​	

# 5、Controller控制器和RestFul风格

## 5.1 Controller控制器

**环境准备：**

1. 新建一个Maven项目，**添加Web支持**，导入依赖；<font color="red">（**记住配置静态资源过滤**）、（**记住要在Project Structure中的Artifacts中导入除了3个Servlet包以外的其他jar包 **）</font>

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
   
   
       <build>
           <resources>
               <resource>
                   <directory>src/main/java</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                   </includes>
                   <filtering>false</filtering>
               </resource>
               <resource>
                   <directory>src/main/resources</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                   </includes>
                   <filtering>false</filtering>
               </resource>
           </resources>
       </build>
   ```

2. 配置**web.xml**文件，**注册DispatchServlet前置控制器**

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
            version="5.0">
   
       <!--1.注册DispatchServlet-->
       <servlet>
           <servlet-name>SpringMVC</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:springmvc-servlet.xml</param-value>
           </init-param>
           <!-- 启动顺序，数字越小，启动越早 -->
           <load-on-startup>1</load-on-startup>
       </servlet>
   
       <!--所有请求都会被springmvc拦截 -->
       <servlet-mapping>
           <servlet-name>SpringMVC</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   
   </web-app>
   ```

3. **添加Spring MVC配置文件**【springmvc-servlet.xml】

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd
          http://www.springframework.org/schema/mvc
          https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
       <!-- 视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
             id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/" />
           <!-- 后缀 -->
           <property name="suffix" value=".jsp" />
       </bean>
   
   </beans>
   ```

4. 视图类【hello.jsp】

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
      <title>Kuangshen</title>
   </head>
   <body>
   测试Controller接口，成功！
   </body>
   </html>
   ```

​			

搭建好环境后，开始对比**实现Controller接口**和**使用注解@Controller**的区别。

​	

### 5.1.1 方式一：实现Controller接口

```java
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

//只要实现了Controller接口，说明这就是一个控制器
public class ControllerTest1 implements Controller {
    @Override
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","Hello World! - ControllerTest1");
        mv.setViewName("test1");
        return mv;
    }
}
```

![image-20220611175901169](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220611175901169.png)

​	

<font color="red">**Spring4.0 开始，不配置处理器映射器，不配置处理器适配器，Spring会使用默认配置来完成工作！**</font>

这种实现接口Controller定义控制器是较老的办法，缺点是：**一个控制器中只有一个方法，如果要多个方法则需**

**要定义多个Controller；定义的方式比较麻烦；**

​	

### 5.1.2 方式二：使用注解@Controller

- `@Controller`注解类型用于声明Spring类的实例是一个控制器

- Spring可以使用扫描机制来找到应用程序中所有基于注解的控制器类，为了保证Spring能找到你的控制器，需

  要在配置文件中声明组件扫描。

  ```xml
  <!-- 自动扫描指定的包，下面所有注解类交给IOC容器管理 -->
  <context:component-scan base-package="com.kuang.controller"/>
  ```

- 增加一个`ControllerTest2`类，使用注解实现；

  ```java
  //@Controller注解的类会自动添加到Spring上下文中，会被Spring接管
  @Controller
  public class ControllerTest2{
  
     //映射访问路径
     @RequestMapping("/t2")
     public String index(Model model){
         //Spring MVC会自动实例化一个Model对象用于向视图中传值
         model.addAttribute("msg", "ControllerTest2");
         //返回视图位置
         return "test";
    }
  }
  ```

​	

**可以发现，两个请求都可以指向一个视图，但是页面结果的结果是不一样的，从这里可以看出视图是被复**

**用的，而控制器与视图之间是弱偶合关系。**

<font color="red">**注解方式是平时使用的最多的方式**</font>

​	

## 5.2 RequestMapping

**@RequestMapping**

- @RequestMapping注解用于映射url到控制器类或一个特定的处理程序方法。可用于类或方法上。用于类上，表

  示类中的所有响应请求的方法都是以该地址作为父路径。

- 为了测试结论更加准确，我们可以加上一个项目名测试 myweb

- 只注解在方法上面

  ```java
  @Controller
  public class TestController {
     @RequestMapping("/h1")
     public String test(){
         return "test";
    }
  }
  ```

  访问路径：http://localhost:8080 / 项目名 / h1

- 同时注解类与方法

  ```java
  @Controller
  @RequestMapping("/admin")
  public class TestController {
     @RequestMapping("/h1")
     public String test(){
         return "test";
    }
  }
  ```

  访问路径：http://localhost:8080 / 项目名/ admin /h1  , 需要先指定类的路径再指定方法的路径。

​		

## 5.3 RestFul风格

**概念**

Restful就是一个资源定位及资源操作的风格。不是标准也不是协议，只是一种风格。基于这个风格设计的软件可以

更简洁，更有层次，更易于实现缓存等机制。

> 相当于传统的风格：http://localhost:8080/hello?method="add"
>
> RestFul风格：http://localhost:8080/hello/add

**功能**

资源：互联网所有的事物都可以被抽象为资源；

资源操作：使用POST、DELETE、PUT、GET，使用不同方法对资源进行操作。

分别对应 添加、 删除、修改、查询。

**传统方式操作资源**  ：通过不同的参数来实现不同的效果！方法单一，post 和 get

​	http://127.0.0.1/item/queryItem.action?id=1 查询，GET

​	http://127.0.0.1/item/saveItem.action 新增，POST

​	http://127.0.0.1/item/updateItem.action 更新，POST

​	http://127.0.0.1/item/deleteItem.action?id=1 删除，GET或POST

**使用RESTful操作资源** ：可以通过不同的请求方式来实现不同的效果！如下：请求地址一样，但是功能可以不同！

​	http://127.0.0.1/item/1 查询，GET

​	http://127.0.0.1/item 新增，POST

​	http://127.0.0.1/item 更新，PUT

​	http://127.0.0.1/item/1 删除，DELETE

**测试**

1. 在新建一个类 RestFulController

   ```java
   @Controller
   public class RestFulController {
   }
   ```

2. 在Spring MVC中可以使用  `@PathVariable` 注解，让方法参数的值对应绑定到一个URI模板变量上。

   ```java
   //RestFul风格的请求：http://localhost:8080/SpringMVC_04_/add2/4/5
       @RequestMapping("/add2/{a}/{b}")
       public String test2(@PathVariable int a, @PathVariable int b, Model model) {
           int res = a + b;
           model.addAttribute("msg", "结果为" + res);
           return "test";
       }
   ```

3. 测试结果

   ![image-20220611230056430](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220611230056430.png)

4. 思考：使用路径变量的好处？

5. - 使路径变得更加简洁；
   - 获得参数更加方便，框架会自动进行类型转换。

6. - 通过路径变量的类型可以约束访问参数，如果类型不一样，则访问不到对应的请求方法，如这里访问是

     的路径是/commit/1/a，则路径与方法不匹配，而不会是参数转换失败。

     ![image-20220611230602363](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220611230602363.png)

7. 修改下对应的String参数类型，再次测试

   ```java
   @RequestMapping("/add4/{a}/{b}")
       public String test4(@PathVariable int a, @PathVariable String b, Model model) {
           String res = a + b;
           model.addAttribute("msg", "结果为" + res);
           return "test";
       }
   ```

   ![image-20220611230338895](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220611230338895.png)

**使用method属性指定请求类型**

用于约束请求的类型，可以收窄请求范围。指定请求谓词的类型如GET, POST, HEAD, OPTIONS, PUT, PATCH, DELETE, 

TRACE等

测试一下：

- 增加一个方法

  ```java
  //映射访问路径,必须是POST请求
  @RequestMapping(value = "/hello",method = {RequestMethod.POST})
  public String index2(Model model){
     model.addAttribute("msg", "hello!");
     return "test";
  }
  ```

- 我们使用浏览器地址栏进行访问默认是Get请求，会报错405：

  ![image-20220611230622970](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220611230622970.png)

- 如果将POST修改为GET则正常了；

  ```java
  //映射访问路径,必须是Get请求
  @RequestMapping(value = "/hello",method = {RequestMethod.GET})
  public String index2(Model model){
     model.addAttribute("msg", "hello!");
     return "test";
  }
  ```

  ![image-20220611230706543](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220611230706543.png)

**小结：**

Spring MVC 的 @RequestMapping 注解能够处理 HTTP 请求的方法, 比如 GET, PUT, POST, DELETE 以及 PATCH。

**所有的地址栏请求默认都会是 HTTP GET 类型的。**

方法级别的注解变体有如下几个：组合注解

```
@GetMapping
@PostMapping
@PutMapping
@DeleteMapping
@PatchMapping
```

`@GetMapping`是一个组合注解，平时使用的会比较多！

它所扮演的是 `@RequestMapping(method =RequestMethod.GET)` 的一个快捷方式。

**<font color = "green">TIP：</font>常用的为简化注解`@RequestMapping("/add2/{a}/{b}")`**。

​	

## 5.4 扩展：小黄鸭调试法

> 场景一：我们都有过向别人（甚至可能向完全不会编程的人）提问及解释编程问题的经历，但是很多时候就
>
> 在我们解释的过程中自己却想到了问题的解决方案，然后对方却一脸茫然。
>
> 场景二：你的同行跑来问你一个问题，但是当他自己把问题说完，或说到一半的时候就想出答案走了，留下
>
> 一脸茫然的你。

其实上面两种场景现象就是所谓的小黄鸭调试法（Rubber Duck Debuging），又称橡皮鸭调试法，它是我们软件工

程中最常使用调试方法之一。

![image-20220611233412547](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220611233412547.png)

此概念据说来自《程序员修炼之道》书中的一个故事，传说程序大师随身携带一只小黄鸭，在调试代码的时候会在

桌上放上这只小黄鸭，然后详细地向鸭子解释每行代码，然后很快就将问题定位修复了。

---

​	

# 6、数据处理及跳转

## 6.1 结果跳转方式

设置ModelAndView对象 , 根据view的名称 , 和视图解析器跳到指定的页面。

> 页面 : {视图解析器前缀} + viewName +{视图解析器后缀}

```xml
<!-- 视图解析器 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
     id="internalResourceViewResolver">
   <!-- 前缀 -->
   <property name="prefix" value="/" />
   <!-- 后缀 -->
   <property name="suffix" value=".jsp" />
</bean>
```

​	

> **ServletAPI**

通过设置ServletAPI , 不需要视图解析器：

1. 通过HttpServletResponse进行输出

2. 通过HttpServletResponse实现重定向

3. 通过HttpServletRequest实现转发

```java
@Controller
public class ModelTest1 {

    @RequestMapping("/m1/t1")
    public String test1(HttpServletRequest request, HttpServletResponse response) {

        HttpSession session = request.getSession();
        System.out.println(session.getId());
        return "test";
    }

}
```

​	

> **SpringMVC**

**方式一：通过SpringMVC来实现转发和重定向 - 无需视图解析器；**

1. **注释掉视图解析器**：

![image-20220612002310664](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220612002310664.png)

2. **Controller类**

   ```java
   @Controller
   public class ModelTest1 {
   
       @RequestMapping("/m1/t2")
       public String test2(Model model) {
           model.addAttribute("msg", "无视图解析器转发   /test.jsp");
           // 转发
           return "/test.jsp";
       }
   
       @RequestMapping("/m1/t3")
       public String test3(Model model) {
           model.addAttribute("msg", "无视图解析器转发2  forward:/index.jsp");
           // 转发2
           return "forward:/test.jsp";
       }
   
       @RequestMapping("/m1/t4")
       public String test4(Model model) {
           model.addAttribute("msg", "无视图解析器重定向  redirect:/index.jsp");
           // 重定向
           return "redirect:/index.jsp";
       }
   }
   ```

​	

**方式二：通过SpringMVC来实现转发和重定向 - 有视图解析器；**

有视图解析器默认是转发，重定向需要特写！

转发会拼接，重定向不拼接

1. **增加视图解析器**

   ```xml
   <!-- 视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
             id="internalResourceViewResolver">
           <!-- 前缀 -->
           <property name="prefix" value="/" />
           <!-- 后缀 -->
           <property name="suffix" value=".jsp" />
       </bean>
   ```

2. **Controller类**

   ```java
   @Controller
   public class ModelTest1 {
   
       @RequestMapping("/m1/t1")
       public String test1(HttpServletRequest request, HttpServletResponse response) {
   
           HttpSession session = request.getSession();
           System.out.println(session.getId());
           return "test";
       }
       
       @RequestMapping("/m2/t1")
       public String test5(Model model) {
           model.addAttribute("msg", "有视图解析器重定向   redirect:/index.jsp");
           //重定向
           return "redirect:/test.jsp";
           //return "redirect:hello.do"; //hello.do为另一个请求/
       }
   ```

​		

## 6.2 数据处理

> **提交的域名称和处理方法的参数名一致**，http://locaohost:8080/user/t1?name=xxxx

【User.java】

```java
public class User {
    private int id;
    private String name;
    private int age;

    public User() {
    }

    public User(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

【UserController.java】

```java
@Controller
@RequestMapping("/user")
public class UserController {
	//提交的域名称和处理方法的参数名一致
    //http://localhost:8080/user/t1?name=XXX
    @RequestMapping("/t1")
    public String test1(String name, Model model) {
        //1. 接收前端参数
        System.out.println("接收到前端的参数为：" + name);
        //2.将返回的结果传递给前端
        model.addAttribute("msg", "接收到前端的参数为：" + name);
        //3. 跳转视图
        return "test";
    }
}
```

![image-20220612102407759](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220612102407759.png)

​	

> **提交的域名称和处理方法的参数名不一致**，http://locaohost:8080/user/t1?username=xxxx
>
> 需要添加注解`@(@RequestParam("")`

```java
    //提交的域名称和处理方法的参数名不一致
    //http://localhost:8080/user/t1?username=XXX
    @RequestMapping("/t2")
    public String test2(@RequestParam("username") String name, Model model) {
        //1. 接收前端参数
        System.out.println("接收到前端的参数为：" + name);
        //2.将返回的结果传递给前端
        model.addAttribute("msg", "接收到前端的参数为：" + name);
        //3. 跳转视图
        return "test";
    }
```

![image-20220612103043624](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220612103043624.png)

​	

> **提交的是一个对象**

实体类【User.java】

```java
public class User {
    private int id;
    private String name;
    private int age;

    public User() {
    }

    public User(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

Controller类【UserController.java】

```java
//前端接收的为一个对象
    @RequestMapping("/t3")
    public String test3(User user) {
        System.out.println(user);
        return "test";
    }
```

![image-20220612103918790](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220612103918790.png)

<font color="red">**说明：如果使用对象的话，前端传递的参数名和对象名必须一致，否则就是null。**</font>

​	

## 6.3 数据显示到前端

> **方式一 : 通过ModelAndView**

【配置版】

```java
public class ControllerTest1 implements Controller {

   public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
       //返回一个模型视图对象
       ModelAndView mv = new ModelAndView();
       mv.addObject("msg","ControllerTest1");
       mv.setViewName("test");
       return mv;
  }
}
```

​		

> **方式二 : 通过ModelMap**

```java
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name, ModelMap model){
   //封装要显示到视图中的数据
   //相当于req.setAttribute("name",name);
   model.addAttribute("name",name);
   System.out.println(name);
   return "hello";
}
```

​	

> **方式三：通过Model**【最常用】

```java
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name, Model model){
   //封装要显示到视图中的数据
   //相当于req.setAttribute("name",name);
   model.addAttribute("msg",name);
   System.out.println(name);
   return "test";
}
```

​	

**对比**

> **Model** 只有寥寥几个方法只适合用于储存数据，简化了新手对于Model对象的操作和理解；
>
> **ModelMap** 继承了 LinkedMap ，除了实现了自身的一些方法，同样的继承 LinkedMap 的方法和特性；
>
> **ModelAndView** 可以在储存数据的同时，可以进行设置返回的逻辑视图，进行控制展示层的跳转。

​	

## 6.4 乱码问题

1. 新建一个表单【form.jsp】

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <form action="<%=request.getContextPath()%>/e/t1" method="post">
       <input type="text" name="name">
       <input type="submit">
   </form>
   </body>
   </html>
   ```

2. 处理类【EncodingController.java】

   ```java
   @Controller
   public class EncodingController {
   
       @RequestMapping("/e/t1")
       public String test1(String name, Model model) {
           model.addAttribute("msg", name);
           return "test";
       }
   }
   ```

3. 编写一个跳转接收页面【test.jsp】

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   ${msg}
   </body>
   </html>
   ```

4. 启动测试，输入中文出现乱码

   ![image-20220612111634228](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220612111634228.png)

​	

以前乱码问题通过过滤器解决，而SpringMVC给我们提供了一个过滤器 , 可以在【web.xml】中配置。

修改了xml文件需要重启服务器！

```xml
<filter>
   <filter-name>encoding</filter-name>
   <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
   <init-param>
       <param-name>encoding</param-name>
       <param-value>utf-8</param-value>
   </init-param>
</filter>
<filter-mapping>
   <filter-name>encoding</filter-name>
   <url-pattern>/*</url-pattern>
</filter-mapping>
```

但是发现，有些极端情况下，这个过滤器对get的支持很不友好。

​	

> **处理办法**

1. 修改Tomcat配置文件，设置编码！

   ```xml
   <Connector URIEncoding="utf-8" port="8080" protocol="HTTP/1.1"
             connectionTimeout="20000"
             redirectPort="8443" />
   ```

   

