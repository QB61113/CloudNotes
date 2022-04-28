# ServletConfig接口

## 一、ServletConfig介绍

- [x] 一个Servlet对象中有一个ServletConfig对象，一对一。100个Servlet对象有100个ServletConfig对象
- [x] Tomcat服务器创建了ServletConfig对象
- [x] 在创建Servlet对象的时候，同时创建了ServletConfig对象

## 二、ServletConfig讲解

- [x] ServletConfig翻译过来为`Servlet对象的配置信息对象`
   - 一个ServletConfig对象就有一个配置信息对象
   - 100个ServletConfig对象就有100个配置信息对象
- [x] ServletConfig对象到底包装了什么信息？
```xml
<servlet>
        <servlet-name>configtest</servlet-name>
<servlet-class>servlet.ConfigTestServlet</servlet-class>
 </servlet>
```

- [x] 获取<servlet-name>
```java
String servletName = config.getServletName()
```

- [x] 配置Servlet对象初始化信息
   - Servlet标签中的`<init-param>`是初始化参数，这个初始化参数会自动被小猫咪封装到ServletConfig对象中
```xml
<init-param>
  <param-name>driver</param-name>
  <param-value>com.mysql.cj.jdbc.Driver</param-value>
</init-param>

<init-param>
  <param-name>user</param-name>
  <param-value>root</param-value>
</init-param>
```

   - 通过ServletConfig对象的两个方法，可以获取到web.xml文件中的初始化参数配置信息
```java
//获取所有的初始化参数的name
java.util.EnumerationMjava.lang.String>getInitParameterNames()  
    

java.lang.String.getInitParameter(java.lang.String.name)
```
## 三、ServletConfig作用

```java
@Override
public void init(ServletConfig config) throws ServletException {
	//获得servlet名称
	String servletName = config.getServletName();
	System.out.println(servletName);
	//获得servelt初始化参数
	String initParameter = config.getInitParameter("url");
	System.out.println(initParameter);
	//获得ServletContext
	ServletContext servletContext = config.getServletContext();
		
		
	}
```
