# web站点的欢迎页面

## 一、什么是一个欢迎页面？

- [x] 对于一个webapp来说，我们是可以设置他的欢迎页面的
- [x] 设置了欢迎页面之后，当你访问这个webapp的时候，或者访问这个web站点的时候，没有指定任何资源路径，这时候会默认访问你的欢迎页面
- [x] 默认的是`http://localhost:8080/Servlet00/login.html`
- [x] 如果我们访问的方式是[http://localhost:8080/Servlet00](http://localhost:8080/Servlet00)，没有具体的资源路径，它默认会访问设置的欢迎页面

## 二、设置一个欢迎页面

### 2.1 设置一个欢迎页面方法

- [x] 在IDEA工具的web目录下新建一个文件login.html
- [x] 在web.xml文件中进行以下的配置
```xml
<welcome-file-list>
        <welcome-file>login.html</welcome-file>
    </welcome-file-list>
```

   - 注意
      - 设置欢迎页面的时候，这个路径不需要以`/`开始，并且这个路径默认是从webapp的根下开始查找
- [x] 启动浏览器，浏览器地址栏输入地址`http://localhost:8080/Servlet05`
- [x] 一个webapp可以设置多个欢迎页
```xml
<welcome-file-list>
	<welcome-file>login.html</welcome-file>
	<welcome-file>page1/page2/page.html</welcome-file>
    </welcome-file-list>
```

   - 注意
      - 越靠上，优先级越高，上面找不到继续往下找

### 2.2 如果在webapp的根目录下还有其他目录

![20220424161529](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424161529.png)

#### （1）方法

- [x] 在webapp根下新建page1
- [x] 在page1下新建一个page2目录
- [x] 在page2目录下新建page.html页面
- [x] 在web.xml文件中应该这样配置
```xml
  <welcome-file-list>
        <welcome-file>page1/page2/page.html</welcome-file>
    </welcome-file-list>
```

   - 注意
      - 路径不需要以`/`开始，并且路径默认从webapp的根下开始找

## 三、特殊的index.html

- [x] 当文件名为index.html的时候，不需要在web.xml文件中配置欢迎页面
   - 原因
      - Tomcat服务器已经提前配好了
   - 实际上配置文件欢迎页面有两个地方可以配置
      - 一个是在webapp内部的web.xml文件中，这个地方配置的属于局部配置
      - 一个是在CATALINA_HOME/conf/web.xml文件中进行配置，这个地方属于全局配置
```xml
<welcome-file-list>
        <welcome-file>index.html</welcome-file>
        <welcome-file>index.htm</welcome-file>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>
```

   - 注意配置原则
      - 局部优先原则（就近原则）

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424161541.png" alt="20220424161541" style="zoom:50%;" />

## 四、欢迎页可以是一个Servlet

- [x] 欢迎页就是一个资源，既然是一个资源，可以是静态，也可以是静态

### 4.1 方法

- [x] 创建一个Servlet类
```java
package servlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;

/**
* @Author: 小雷学长
* @Date: 2022/3/20 - 16:23
* @Version: 1.8
*/
public class WelcomeServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException {
        
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.print("<h1>Welcome to Serclet</h1>");
    }
    
}

```

- [x] 在web.xml文件中配置Servlet和欢迎页
```java
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    
   <!--配置欢迎页-->
    <welcome-file-list>
        <welcome-file>welcome</welcome-file>
    </welcome-file-list>
    
    <!--配置Servlet-->
    <servlet>
        <servlet-name>WelcomeServlet</servlet-name>
        <servlet-class>servlet.WelcomeServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>WelcomeServlet</servlet-name>
        <url-pattern>/welcome</url-pattern>
    </servlet-mapping>

</web-app>
```
![20220424161558](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424161558.png)
