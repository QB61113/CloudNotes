# 获取请求URI

## 一、方法

- ✅`//获取请求的URI`

​        `String requestURI = request.getRequestURI();  `

​        `System.out.println(requestURI);`

## 二、实现

```java
public class RequestTestServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {

        //获取请求的URI
        String requestURI = request.getRequestURI();
        System.out.println(requestURI);

        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.print(requestURI);

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



![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424162852.png)