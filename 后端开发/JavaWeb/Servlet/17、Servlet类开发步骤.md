# Servlet类开发步骤

- [x] 编写一个Servlet类，直接继承HttpServlet
- [x] 重写doGet方法或者重写doPost方法，到底写谁？Javaweb程序员说了算
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
* @Date: 2022/3/20 - 15:24
* @Version: 1.8
*/
public class LoginServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
        throws ServletException, IOException {
        
        resp.setContentType("text/html");
        PrintWriter out = resp.getWriter();
        out.print("<h1>登录成功……</h1>");
    }
}

```

- [x] 将Servlet类配置到web.xml文件当中
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>login</servlet-name>
        <servlet-class>servlet.LoginServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>login</servlet-name>
        <url-pattern>/login</url-pattern>
    </servlet-mapping>
</web-app>
```

- [x] 准备前端的页面（form表单），form表单中指定请求路径路径即可，
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Login page</title>
  </head>
  <body>
    <form action="/Servlet00/login" method="post">
      <input type="submit" value="login">
    </form>
    
  </body>
</html>
```
![image-20220523000515128](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220523000515128.png)

![20220424161324](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424161324.png)

