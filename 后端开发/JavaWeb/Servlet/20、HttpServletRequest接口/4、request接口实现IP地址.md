# request接口实现IP地址

```java
package servlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;
import java.net.HttpCookie;

/**
 * @Author: 小雷学长
 * @Date: 2022/3/20 - 17:22
 * @Version: 1.8
 */
public class RequestTestServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {

        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.print("");

        //获取客户端的IP地址
        String remoteAddr = request.getRemoteAddr();
        System.out.println("客户端的IP地址:" + remoteAddr);

    }


}
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
  
  <servlet>
    <servlet-name>RequestTestServlet</servlet-name>
    <servlet-class>servlet.RequestTestServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>RequestTestServlet</servlet-name>
    <url-pattern>/testrequest</url-pattern>
  </servlet-mapping>
  
</web-app>
```
<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424162150.png" alt="img" style="zoom: 50%;" />
<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424162159.png" alt="img" style="zoom:50%;" />

