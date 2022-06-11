# 适配器模式改造Servlet

## 一、引入适配器

- [x] 我们编写service方法，其他大部分情况下是不需要使用的，代码很丑陋

## 二、适配器设计模式Adapter

### 2.1 实例

- [x] 手机连直接插到220V的电压下，手机就报废了，找一个充电器，这个充电器就是一个适配器。手机连接适配

  器，适配器连接220V的电压，这样问题就解决了

### 2.2 方法

- [x] 编写一个GenericServlet类，这个类是一个抽象类，其中有一个抽象方法service
- [x] GenericServlet实现Servlet接口
- [x] GenericServlet是一个是适配器
- [x] 以后编写所有的Servlet类继承GenericServlet，重写service方法即可
```java
package servlet;

import jakarta.servlet.*;

import java.io.IOException;

/**
 * @Author: 小雷学长
 * @Date: 2022/3/17 - 19:47
 * @Version: 1.8
 */

/**
 * 编写一个标准通用的Servlet，起名：GenericServlet
 * 以后所有的Servlet类都不要直接实现Servlet接口了
 * 以后所有的Servlet类都要继承GenericServlet类
 * GenericServlet 就是一个适配器
 */
public abstract class GenericServlet implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    /**
     * 抽象方法，这个最常用,所以要求子类必须实现service方法
     * @param servletRequest
     * @param servletResponse
     * @throws ServletException
     * @throws IOException
     */
    @Override
    public abstract void service(ServletRequest servletRequest, ServletResponse servletResponse)
            throws ServletException, IOException;


    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}

```
```java
package servlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.ServletRequest;
import jakarta.servlet.ServletResponse;

import java.io.IOException;

/**
 * @Author: 小雷学长
 * @Date: 2022/3/17 - 21:03
 * @Version: 1.8
 */
public class VipServlet extends GenericServlet {
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse)
            throws ServletException, IOException {
        System.out.println("VIP");
    }
}
```
```java
package servlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.ServletRequest;
import jakarta.servlet.ServletResponse;

import java.io.IOException;

public class LoginServlet extends GenericServlet{

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("Login");
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
        <servlet-name>loginServlet</servlet-name>
        <servlet-class>servlet.LoginServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>loginServlet</servlet-name>
        <url-pattern>/login</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>VIPServlet</servlet-name>
        <servlet-class>servlet.VipServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>VIPServlet</servlet-name>
        <url-pattern>/VIP</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>aservlet</servlet-name>
        <servlet-class>servlet.AServlet</servlet-class>
        <!--<load-on-startup>0</load-on-startup>-->
    </servlet>
    <servlet-mapping>
        <servlet-name>aservlet</servlet-name>
        <url-pattern>/a</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>bservlet</servlet-name>
        <servlet-class>servlet.BServlet</servlet-class>
        <!--<load-on-startup>1</load-on-startup>-->
    </servlet>
    <servlet-mapping>
        <servlet-name>bservlet</servlet-name>
        <url-pattern>/b</url-pattern>
    </servlet-mapping>
</web-app>
```
## 三、改造GenericServlet

### 3.1 关于init方法

- [x] 在提供了GenericServlet之后，init方法仍然会被调用
- [x] init方法是Tomcat服务器调用的
- [x] Tomcat服务器先创建了ServletConfig对象，如何调用init方法，将ServletConfig对象传给了init方法

