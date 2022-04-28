# ServletContext接口

## 一、ServletContext的获取

- [x] 通过ServletConfig获取ServletContext接口
```java
//第一种方式
ServletContext application = config.getServletContext();

//第二种方式:通过this也可以获得ServletContext对象
ServletContext application2 = this.getServletContext();
```
## 二、ServletContext的作用

- [x] 获取 web.xml 中配置的上下文参数 `context-param–context.getInitParameter()`
- [x] 获取当前的工程路径`context.getContextPath()`
- [x] 获取工程部署后在服务器硬盘上的绝对路径`context.getContextPath()`
- [x] 像 Map 一样存取数据`setAttribute()、getAttribute()、removeAttribute()`

## 三、重点

- [x] 以后在编写Servlet类的时候，实际上不是会去直接继承ServletContext类的，直接继承的是HttpServlet。（因为HttpServlet是HTTP协议专用的
- [x] 继承结构
```java
jakarta.servlet.Servlet（接口）【爷爷】
jakarta.servlet.GenericServlet implements Servlet（抽象类）【儿子】
jakarta.servlet.http.HttpServlet extends GenericServlet（抽象类）【孙子】
```
