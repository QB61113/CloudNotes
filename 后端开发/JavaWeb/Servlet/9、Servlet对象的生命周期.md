# Servlet对象的生命周期

## 一、什么是Servlet对象生命周期？

- Servlet对象是什么时候被创建？
- Servlet对象是什么时候被销毁？
- Servlet对象创建了几个？
- Servlet对象的生命周期表示：一个Servlet对象从出生到最后的死亡，整个过程是怎样的？
- Servlet对象是由谁来维护的？

## 二、Servlet对象生命周期

### 2.1 Servlet对象和WEB容器

- Servlet对象的创建，对象上方法的调用，对象最终的销毁，程序员是无权干预的
- Servlet对象的生命周期是由Tomcat服务器（WEB Sever）全权负责的
- Tomcat服务器通常我们又称为：WEB容器
- 我们自己new的Servlet对象是不受WEB容器管理的
- WEB容器创建的Servlet对象，这些是Servlet对象都会被放到一个集合当中（HashMap），只有放到这个HashMap集合中的Servlet才能够被WEB容器管理，自己new的Servlet的对象不会被WEB容器管理，（自己new的Servlet独享不在容器当中）
- WEB容器底层应该都有一个HashMap这样的集合，在这个集合当中存储了Servlet对象和请求路径之间的关系

### 2.2 Servlet对象生命周期

```java
package servlet;

import jakarta.servlet.*;

import java.io.IOException;

/**
 * @Author: 小雷学长
 * @Date: 2022/3/17 - 10:37
 * @Version: 1.8
 */
public class AServlet implements Servlet {

    //无参数构造方法
    public AServlet() {
        System.out.println("A无参数构造方法启动了");
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("A service启动了");
    }

    @Override
    public void destroy() {
        System.out.println("A destory启动了");

    }

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println("A inti启动了");
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

}

```

- 默认情况下服务器启动时 A Servlet对象并没有被实例化
- 用户发送第一次请求的时候，控制台输出了以下内容：
```
A无参数构造方法启动了
A inti启动了
A service启动了
```

- 根据以上输出的内容得出结论：
   1. 用户在发送第一次请求的时候Servlet对象被实例化（AServlet构造的方法被执行了。并且执行的是无参数构造方法）
   1. A Servlet对象被创建出来之后，Tomcat服务器马上调用了AServlet对象的init方法（init方法在执行的时候，Servlet对象已经存在了，已经被创建出来了）
   1. 无参数构造方法、init方法只在第一次用户发送请求的时候执行，也就是说无参数构造方法只执行一次，init方法只被Tomcat服务器调用一次
   1. 只要用户发送一次请求，service方法必然只会被Tomcat定位器调用一次，发送100次请求，service方法会被调用100次
   1. 关闭服务器的时候，控制台输出了以下内容
```
A destory启动了
```

   - 通过以上输出内容，可以得出一下结论：
      - Servlet的destroy方法只被Tomcat服务器用一次
      - destroy方法在服务器关闭的时候被调用，因为服务器关闭的时候要销毁AServlet对象的内存
      - 服务器在销毁A Servlet对象内存之前，Tomcat服务器会自动调用AServlet对象的destroy方法
      - destroy方法执行的时候A Servlet对象还在，没有被销毁，destroy方法执行结束之后，AServlet对象的内存才会被释放

### 2.3 Servlet生命周期总结

#### （1） 各方法的含义

- 无参数构造方法执行：标志着出生
- init方法的执行：标志着正在接收教育
- service方法的执行：标志着开始工作，并且一直服务
- destroy方法的执行：标志着临终

#### （2）各方法被调用的次数

- 构造方法只执行一次
- init方法只执行一次
- service方法：用户发送一次请求一次，发送N次请求N次
- destroy方法只执行一次

#### （3）常见错误

- 当Servlet类中编写一个有参数构造方法，如果没有手动编写无参数构造方法会报错，`500错误---->500错误是一个HTTP协议的错误状态码`，一般情况下是因为服务器端的Java程序出现了异常（服务器内部错误都是500错误）
```java
/*  //无参数构造方法
    public AServlet() {
        System.out.println("A无参数构造方法启动了");
    }*/

    //程序员手动提供有参数的构造方法
    public AServlet(String s){

    }
```
![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424155715.png)

- 如果没有无参数的构造方法会导致500错误
- 一般不建议程序员来定义构造方法

## 三、研究

```java
package servlet;

import jakarta.servlet.*;

import java.io.IOException;

/**
 * @Author: 小雷学长
 * @Date: 2022/3/17 - 10:37
 * @Version: 1.8
 */
public class AServlet implements Servlet {

    //无参数构造方法
    public AServlet(){
        System.out.println("无参数构造方法");

    }
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {

    }

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

import jakarta.servlet.*;

import java.io.IOException;

/**
 * @Author: 小雷学长
 * @Date: 2022/3/17 - 11:03
 * @Version: 1.8
 */
public class BServlet implements Servlet {

    //无参数构造方法
    public BServlet() {
        System.out.println("这是B无参数构造");
    }

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {

    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

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
        <servlet-name>aservlet</servlet-name>
        <servlet-class>servlet.AServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>aservlet</servlet-name>
        <url-pattern>/a</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>bservlet</servlet-name>
        <servlet-class>servlet.BServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>bservlet</servlet-name>
        <url-pattern>/b</url-pattern>
    </servlet-mapping>
</web-app>
```

1. 测试发现，服务器在启动时Servlet对象不会被实例化
1. 这个设计是合理的，用户没有发送请求之前，如果提前创建出来所有的Servlet对象，必然是消耗内存的，并且一直没有用用户访问，显然这个Servlet对象是一个废物，没必要先创建

### 3.1 如何让服务器启动的时候创建Servlet对象

1. 在Servlet标签中添加`_<_load-on-startup_>_0_</_load-on-startup_>_`子标签，在该子标签中，越小的整数优先级越高
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>aservlet</servlet-name>
        <servlet-class>servlet.AServlet</servlet-class>
        <load-on-startup>0</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>aservlet</servlet-name>
        <url-pattern>/a</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>bservlet</servlet-name>
        <servlet-class>servlet.BServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>bservlet</servlet-name>
        <url-pattern>/b</url-pattern>
    </servlet-mapping>
</web-app>
```
![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424155753.png)

2. 一般不配，特殊情况下才配

## 四、各方法的使用

### 4.1 service方法

- [x] service方法是使用最多的方法，service方法一定要实现的，因为service方法是处理用户请求的核心方法

### 4.2 init方法

- [x] init方法很少用，通常在init方法中当初始化操作，并且这个初始化操作只需要执行一次，例如：数据库连接池，初始化线程池……

### 4.3 destroy方法

- [x] destroy方法也很少用，通常在destroy方法中，进行资源的关闭，马上对象要被销毁时，抓紧时间关闭资源，还有什么资源没保存的，抓紧时间保存一下

### 4.4 无参数构造方法能不能代替init方法？

- [x] 不能。
- [x] Servlet规范中要求，作为Javaweb程序员，编写Servlet类的时候，不建议手动编写构造方法，很容易让无参数构造方法消失，这个操作可能会导致Servlet对象无法实例化。
- [x] 所以init方法的存在是必要的
