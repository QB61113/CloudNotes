# SpringBoot

# 1 SpringBoot简介

> **回顾Spring**

Spring是一个开源框架，2003 年兴起的一个轻量级的Java 开发框架，作者：Rod Johnson 。

**Spring是为了解决企业级应用开发的复杂性而创建的，简化开发。**

​	

> **Spring**

为了降低Java开发的复杂性，Spring采用了以下4种关键策略：

1、基于POJO的轻量级和最小侵入性（非入侵式）编程，所有东西都是bean；

2、通过IOC，依赖注入（DI）和面向接口实现松耦合；

3、基于切面（AOP）和惯例进行声明式编程；

4、通过切面和模版减少样式代码，`RedisTemplate`，`xxxTemplate`；

​	

> **什么是SpringBoot**

在Javaweb时，最开始是用Servlet结合Tomcat，跑出一个Hello World程序！是要经历特别多的步骤，后来到了后来

就用了框架Struts，再后来是SpringMVC，到了现在的SpringBoot，过一两年又会有其他web框架出现；

**SpringBoot**，就是一个javaweb的开发框架，和SpringMVC类似，对比其他javaweb框架的好处，官方说是简化开

发，**约定大于配置**， 能迅速的开发web应用，几行代码开发一个http接口。

Spring Boot 以<font color="red">**约定大于配置的核心思想**</font>，默认帮我们进行了很多设置，多数 Spring Boot 应用只需要很少的 Spring

 配置。同时它集成了大量常用的第三方库配置（例如 Redis、MongoDB、Jpa、RabbitMQ、Quartz 等等），Spring

Boot 应用中这些第三方库几乎可以零配置的开箱即用。

简单来说就是SpringBoot其实不是什么新的框架，它默认配置了很多框架的使用方式，就像maven整合了所有的jar

包，spring boot整合了所有的框架 。Spring Boot 出生名门，从一开始就站在一个比较高的起点，又经过这几年的

发展，生态足够完善，Spring Boot 已经当之无愧成为 Java 领域最热门的技术。

​	

**Spring Boot的主要优点：**

- 为所有Spring开发者更快的入门
- **开箱即用**，提供各种默认配置来简化项目配置
- 内嵌式容器简化Web项目
- 没有冗余代码生成和XML配置的要求

---

​	

# 2 第一个SpringBoot程序

## 2.1 Hello SpringBoot

> **Hello，World**

环境准备：

- jdk 1.8
- Maven 3.8.1
- SpringBoot 2.x 最新版

开发工具：

- IDEA

​	

> **创建基础项目说明**

Spring官方提供了非常方便的工具让我们快速构建应用。

传送门：[Spring Initializr 快速构建应用](https://start.spring.io/ "点击快速构建应用")

![image-20220618173850685](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220618173850685.png)



**创建方式一：**使用Spring Initializr 的 Web页面创建项目

1、打开  [https://start.spring.io/](https://start.spring.io/)

2、填写项目信息

3、点击`Generate Project`按钮生成项目；下载此项目

4、解压项目包，并用IDEA以Maven项目导入，一路下一步即可，直到项目导入完毕。

5、如果是第一次使用，可能速度会比较慢，包比较多、需要耐心等待一切就绪。

​	

**创建方式二：**直接在IDEA中创建项目

1、创建一个新项目

2、选择`spring initalizr`， 可以看到默认就是去官网的快速构建工具那里实现

3、填写项目信息

4、选择初始化的组件（初学勾选 Web 即可）

5、填写项目路径

6、等待项目构建成功

​	

**项目结构分析：**

通过上面步骤完成了基础项目的创建。就会自动生成以下文件。

1、程序的主启动类`HelloWorldApplication`，<font color="green">**他是程序的主入口，不能删也不能改！！！**</font>

2、一个 `application.properties` 配置文件，<font color="green">**他是SpringBoot的核心配置文件**</font>

3、一个 单元测试类`HelloWorldApplicationTests`

4、一个 pom.xml

![image-20220618182750159](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220618182750159.png)

​	

> **分析一下pom.xml**

```xml
<!-- 父依赖 -->
<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.7.0</version>
		<relativePath/> <!-- lookup parent from repository -->
</parent>

<dependencies>
        <!-- web场景启动器, web依赖: Tomcat, DispatcherServlet, xml....... -->-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<!-- springboot单元测试 -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
             <!-- 剔除依赖 -->
             <exclusions>
             <exclusion>
                <groupId>org.junit.vintage</groupId>
                <artifactId>junit-vintage-engine</artifactId>
            </exclusion>
        </exclusions>
		</dependency>
	</dependencies>

	<build>
		<plugins>
            <!-- 打包插件 -->
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
```

​	

> **编写一个Http接口**

1、在**主程序的同级目录下**，新建一个`controller`包，<font color="red">**一定要在主程序同级目录下，否则识别不到**</font>

![image-20220618183731285](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220618183731285.png)

2、在包中新建一个`HelloController`类，用于调用业务，接收前端参数

```java
//本身就是一个Spring组件

@RestController
public class HelloController {

    //用于调用业务，接收前端参数
    @RequestMapping("/hello")
    public String hello() {
        return "Hello World!";
    }
}
```

3、编写完毕后，从主程序启动项目`HelloWorldApplication`，浏览器发起请求，看页面返回；控制台输出了 Tomcat 访问的端口号！

![image-20220618184054478](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220618184054478.png)

​	

> **将项目打成jar包**

:mag:点击Maven的`Lifecycle`下的`package`

如果打包成功，则会在target目录下生成一个 jar 包，就可以在任何地方运行了！

如果遇到错误，可以配置打包时跳过项目运行测试用例。

```xml
<!--
    在工作中,很多情况下我们打包是不想执行测试用例的
    可能是测试用例不完事,或是测试用例会影响数据库数据
    跳过测试用例执
    -->
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <!--跳过项目运行测试用例-->
        <skipTests>true</skipTests>
    </configuration>
</plugin>
```

​	

<font color="green" >**所以SpringBoot的核心原理是：自动装配**</font>

​	

## 2.2 修改端口号

在**resources目录下**的**application.properties**添加：

```properties
#更改项目端口号
server.port=8081
```

![image-20220618192330997](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220618192330997.png)

​	

## 2.3 修改Banner

在项目下的 **resources 目录**下新建一个**banner.txt** 即可。

图案可以到：[banner图案](https://www.bootschool.net/ascii "点击到banner图案网站") 这个网站生成，然后拷贝到文件中即可！

---

​	

# 3

