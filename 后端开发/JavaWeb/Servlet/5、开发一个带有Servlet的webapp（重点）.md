# 开发一个带有Servlet的webapp（重点）

## 一、开发步骤

1. 在webapp目录下新建一个目录，起名crm（这个crm就是webapp的名字）。当然也可以是其他项目，比如银行系统可以创建一个目录bank，办公系统可以创建一个oa
   - 注意：
      - crm就是这个webapp的根
2. 在webapp根目录（crm）下，创建一个目录：WEB-INF
   - 注意：
      - 这个目录的名字是Servlet规范中规定的，必须全部大写，必须一模一样
3. 在WEB-INF目录下新建一个目录：classes
   - 注意：
      - 这个目录的名字必须是全部小写的classes。这也是Servlet规范中规定的，另外这个目录下一定存放的是java程序编译之后的class文件（这里存放的是字节码文件）
4. 在WEB-INF目录下新建一个目录：lib
   - 注意：
      - 这个目录不是必须的，但如果一个webapp需要第三方的jar包的话，这个jar包要放到这个lib下，这个目录的名字也不能随意编写，必须全部小写的lib。例如java语言连接数据库需要数据库的驱动jar包，那么这个jar包一定要放在lib目录下，这个也是Servlet规范中规定的
5. 在WEB-INF目录下新建一个_文件_：web.xml
   - 注意：
      - 这个文件是必须的，这个文件名字必须叫web.xml。这个文件必须放在这里，一个合法的webapp，web.xml是必须的，这个web.xml文件就是一个配置文件，在这个配置文件中描述了请求路径和Servlet类之间的对照关系
      - 这个文件最好从其他的webapp中拷贝，最好别手写，复制粘贴
6. 编写一个Java程序，这个小Java必须实现Servlet接口。
   - 思考？
      - 这个Servlet接口不在jdk中（因为jServlet不是JavaSE了，Servlet属于JavaEE）
      - Servlet接口（Servlet.class文件）是Oracle提供的
      - Servlet接口是JavaEE的规范中的一员
      - Tomcat服务器实现了Servlet规范，所以Tomcat服务器也需要使用Servlet接口，Tomcat服务器中应该有这个接口，Tomcat服务器的CATALINA_HOME\lib目录下有一个servlet-api.jar，解压后会得到一个Servlet.class文件
      - 从JakartaEE9开始，Servlet接口的全名变了：jakarta.servlrt.Servlet
   - 注意：
      - 编写这个Java程序的时候，java源码在哪就在哪，位置无所谓，只需要将Java源代码编译之后的class文件放到classes目录下即可
7. 编译编写的HelloServlet
   - 重点：
      - 配置环境变量CLASSPATH=.;路径下的lib\servlet-api.jar
      - 配置环境变量CALSSPATH与没有任何关系，只是为了让java文件成功编译
8. 将以上编译之后的HelloServlet.clss文件拷贝到WEB-INF\classes目录下
8. 在web.xml文件中编写配置信息，让“请求路径”和“Servlet类名”关联在一起
   - 这一步用专业属于描述：在web.xml文件中注册Servlet类
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee 
                             https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
         version="5.0"  
         metadata-complete="true">
  
         <!--servlet描述信息-->
         <!--任何一个Servlet都对应一个servlet-mapping-->      
         <servlet>
            <servlet-name>asdfghjk</servlet-name>
            <servlet-class>Servlet.HelloServlet</servlet-class>
         </servlet>
  
          <!--servlet映射信息-->
         <servlet-mapping>
             <!--这个也是随便的，不过这里写的内容要和上面的一样-->
            <servlet-name>asdfghjk</servlet-name>
           <!---这里需要一个路径-->
            <!---这个路径唯一的要求是必须以 / 开始-->
            <!--当前这个路径可以随便写-->
            <url-pattern>/as/fe/g/h/gh/h</url-pattern>
         </servlet-mapping>
         </web-app>

```

10. 启动Tomcat服务器
10. 打开浏览器，在浏览器地址栏输入一个url，这个URL必须是web.xml文件中的url-pattern一致
   - 浏览器上编写的路径太复杂，可以使用超链接
   - **非常重要：**
      - html页面只能放到WEB-INF目录之外
      - 以后不需要编写main方法了，Tomcat服务器负责调用main方法，Tomcat服务器启动时候，执行的就是main方法，JavaWeb程序员只需要编写Servlet接口实现类，然后将其注册到web.html文件中即可

## 二、关于JavaEE的版本

- 目前最高版本是JavaEE8
- JavaEE被Oracle捐献给了Apache
- Apache把JavaEE改成了JakartaEE
- 以后没有JavaEE了，以后都叫Jakarta EE
- JavaEE8版本升级以后的“JavaEE9”不再是这个名字了，叫做JakartaEE9
- JavaEE8的时候对应的Servlet类名是：javax.servlet.Servlet
- JavaEE9的时候对应的Servlet类名是：jakarta.servlet.Servlet

## 三、向浏览器响应一段HTML代码

```java
package Servlet;

import jakarta.servlet.Servlet;
import jakarta.servlet.ServletException;
import jakarta.servlet.*;

import java.io.IOException;


import java.io.IOException;
import java.io.PrintWriter;


public class HelloServlet implements Servlet {


    public void init(ServletConfig config) throws ServletException {

    }

    public void service(ServletRequest request, ServletResponse response)
            throws ServletException, IOException {

        //这里往控制台输出
        System.out.println("Hello word!");

        //设置响应的内容类型是普通文本或html代码
        //注意:这是响应的内容类型时，不要在获取流之后设置
        //需要在设置流对象之前使用，有效
        response.setContentType("text/html");
        //若出现乱码:加上charset=utf-8
        //response.setContentType("text/html;charset=utf-8");

        //怎么将一个信息直接输入到浏览器上？？？
        //需要使用ServletResponse接口:response
        //response表示响应向浏览器发送数据叫做响应
        PrintWriter out = response.getWriter();

        //向浏览器上打印
        out.print("Hello Servlet,You are first servlet!");

        //在浏览器上输出一串代码
        out.print("<h1>hello servlet</h1>");

        //这是一个输出流，负责输出数据或字符串到浏览器
        //这个输出流不需要我们刷新，不需要我们关闭，这些都由Tomcat完成
        /*
        out.flush();
        out.close();
         */
    }

    public void destroy() {

    }

    public String getServletInfo() {
        return "";
    }

    public ServletConfig getServletConfig() {
        return null;
    }


}

```
```html
<!doctype html>
<html>
<head>
	<title>index page</title>
</head>
<body>
  <!--<a href="请求路径">
请求路径要带项目名-->
<a href="http://127.0.0.1:8080/crm/as/fe/g/h/gh/h">hello</a>
</body>
</html>
```
## 3.1 请求路径

- [x] 请求路径要带上项目名
   - IDEA中

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424151013.png" alt="20220424151013" style="zoom: 33%;" />

## 四、总结

> 一个合法的webapp目录结是什么样的？

```xml
webapproot
	|…………WEB-INF
		|…………classes（存放字节码）
		|…………lib（第三方jar包）
		|…………web.xml（注册Servlet）
	|…………html
	|…………css
	|…………JavaScript
	|…………image
……
```
