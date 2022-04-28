# 使用IDEA工具开发Servlet

##  一、使用IDEA开发Servlet关键步骤

### 1.1 关键步骤

1. 让module变成JavaEE的模块
   1. 在Module点击右键：Add Framework Suppoet……（添加框架支持）
   1. 在弹出的窗口中选择Web Application（选择的是webapp的支持）
   1. 选择了这个webapp的支持之后，IDEA会自动生成一个符合Servlet规范的webapp目录结构
   1. 注意：
      - 在IDEA工具中根据Web Application模板生成的目录中有一个web目录，这个web目录就代表webapp的根
2. 若发现Servlet.class文件没有，则将CATALINA_HOME/lib/servlet-api.jar和jsp-api.jar添加到classpath中（这里的classpath是IDEA的classpath）
   1. IDEA中File---->Project Structure……---->modules---->Dependencies---->增加一个Jars---->添加servlet-api.jar和jsp-api.jar
   1. 实现jakarta.servlet.Serlvet接口的5个方法
3. 在WEB-INF目录下新建一个子目录：lib
   1. 将数据库驱动jar包拷贝到lib目录下
4. 在web.xml文件中完成StudentServlet类的注册（请求路径和Servlet之间对应起来）
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    
    <servlet>
        <servlet-name>studentSerlvet</servlet-name>
        <servlet-class>Servlet.StudentServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>studentSerlvet</servlet-name>
        <url-pattern>/serfdsfdfsdf</url-pattern>
    </servlet-mapping>
</web-app>
```

5. 给一个html页面，在HTML页面中编写一个超链接，用户点击这个超链接，发送请求，Tomcat执行后台的StudentServlet
   - student.html
      - 这个文件不能放到WEB-INF目录里面，只能放到WEB-INF目录外面
      - student.html的内容
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>student page</title>
</head>
<body>
<!--这里的项目名是 /xmm ，无法动态获取，先写死-->
<a href = "/xmm/servlet/student">点击这里</a>

</body>
</html>
```

6. 让IEDA工具去关联Tomcat服务器。关联过程中将webapp部署到Tomcat服务器中
   1. IDEA工具右上角Add COnfiguration
   1. 左上角加号，点击Tomcat Servlet---->local
   1. 在弹出的界面中设置服务器sever参数（基本不用动）
   1. 在当前窗口中有一个Deployment（点击这个部署webapp），继续点击加号---->Artificial……，部署即可
   1. 修改Application context（应用的根）为：空
7. 启动Tomcat服务器
   1. 在右上角有一个绿色的小虫子（Debug），点击可以采用Debug模式启动Tomcat服务器
   1. 在开发模式中建议使用Debug模式启动Tomcat服务器
8. 打开浏览器，在浏览器地址栏输入：http://localhost:8080/student.html
8. html页面只能放到WEB-INF目录之外
