# HttpServlet源码分析

## 一、HttpServlet类

- [x] HttpServlet类是专门为HTTP协议准备的，比GenericServlet更适合HTTP协议的开发
- [x] HttpServlet在哪个包下？
   - jakarta.serlvet.http.HttpServlet

## 二、Servlet规范中的接口

- [x] jakarta.servlet.Servlet              核心接口（接口）
- [x] jakarta.serlvet.SerlvetContext  Servlet上下文接口（接口）
- [x] jakarta.servlet.ServletConfig    Servlet配置信息接口（接口）
- [x] jakarta.servlet.ServletRequest  Servlet请求接口（接口）
- [x] jakarta.servlet.ServletResponse   Servlet响应接口（接口）
- [x] jakarta.servlet.ServletException   Servlet异常（类）
- [x] jakarta.servlet.GenericServlet      标准通用的Servlet类（抽象类）

## 三、http包下都有哪些类和接口 

>jakarta.servlet.http.*;

- [x] jakarta.servlet.http.HttpServlet（Http协议专用的Servlet类，抽象类）
- [x] jakarta.servlet.http.HttpServletRequest（Http协议专用的请求对象）
- [x] jakarta.servlet.http.HttpServletResponse（Http协议专用的响应对象）

## 四、HttpServletRequest对象封装了什么信息？

- [x] HttpServletRequest，简称request对象
- [x] HttpServletRequest中封装了请求协议的全部内容

## 五、HttpServlet源码分析

- [x] 编写的HelloServlet直接继承HttpServlet，直接重写HttpServlet类中的Service()方法行吗？
   - 可以，只不过是享受不到405错误，享受不到HTTP协议专属的东西

