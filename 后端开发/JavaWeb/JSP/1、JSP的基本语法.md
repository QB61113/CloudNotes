# JSP的基本语法

## 一、在Servlet中编写前端代码的问题

⚠️Java程序中编写前端代码，编写难度大，麻烦

⚠️Java程序中编写前端代码，显然程序的耦合度非常高

⚠️Java程序中编写前端代码，代码非常不美观

⚠️Java程序中编写前端代码，维护成本太高（不容易发现错误）

⚠️修改一个小小的前端代码，只要有改动，就需要重新编译Java代码，生成新的class文件，打一个新的war包，重新发布



## 二、理解JSP

:mag:JSP就是Java程序（JSP的本质就是Servlet）

:mag:JSP是JavaServletPages的缩写（基于Java语言实现的服务器端的页面）

:mag:JSP也是JavaEE中13中规范之一

:mag:每一个web容器/web服务器都会内置一个JSP翻译引擎

:mag:jsp文件在WEB-INF目录之外，在webapp的根目录下

:mag:实际上访问`http://localhost:8080/jsp/index.jsp`这个index.jsp文件底层执行的是：index_jsp.class文件这个程序

:mag:这个index.jsp会被tomcat翻译生成index_jsp.java文件，然后tomcat服务器又将index.jsp编译生成index_jsp.class文件

:mag:访问index.jsp实际上执行的是index_jsp.class中的方法

:mag:JSP实际上是一个Servlet

+ index.jsp访问的时候，会自动翻译生成index_jsp.java，再自动百编译生成index_jsp.class，那么index_jsp这就是一个类

- index_jsp类继承HttpServlet
- jsp的生命周期和Servlet的生命周期完全相同

:mag:JSP文件第一次访问比较慢，因为大部分运维人员在演示项目的时候，会提前把所有的jsp文件都访问一遍

:mag:jsp文件中直接编写文字，被翻译到servlet类的service方法的out.writer("翻译到这里")，直接翻译到双引号里，打印输出到浏览器



## 三、JSP的基础语法和输出语句

### 3.1 JSP的基础语句

`< %Java语法; %>`

- > 在这个符号中编写的视为Java程序，被翻译到Servlet类的service方法内部

- > `<%  %>`在这个里面写Java的时候，要时刻记住是在方法体中写代码

![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424214505.png)



### 3.2 JSP专业注释

```jsp
<!-- HTML的注释，这个注释不专业，仍然会被翻译到Java源码中，在JSP中不要使用这种注释-->
<%--JSP专业，这个注释信息不会被翻译到Java源码中，建议使用这种注释信息--%>
```

:mag:在Service方法中不能使用private等访问权修饰变量，`<%  %> `这个相当于是Java里的方法，不能在方法体中编写静态代码块，不能再方法体中写方法，不能方法套方法；

:mag:在JSP中`<% %>`可以出现多个；

:mag:每一行都是Java语句，要符合Java规范；

:mag:`<%!  %>`这个符号编写的Java代码块会被翻译到service方法之外，这个很少用。因为：

<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424214644.png" alt="img" style="zoom:33%;" />



### 3.3 JSP输出语句

```jsp
<%  String name = "jack"; out.write("name1 = " + name);%>
```

⚠️以上代码中的==out==是JSP的==九大内置对象之一==，可以直接拿来用，当然必须==只能在service方法内部使用==

![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424214826.png)

✅如果向浏览器上输出的内容没有Java代码，可以直接在JSP里编写，不需要写到`<% %>`里

![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424214847.png)

✅如果输出的内容含有java代码，这个时候可以使用一下格式的语法

- `<% = %>`注意：在`=`后面编写要输出的内容相当于`out.print();`

- ==当输出的是一个动态变量时使用，因为输出的内容可以直接在JSP里编写==

![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424214900.png)

- - 所以`<% = %>`最终被翻译成了java代码：`out.print();`



### 3.4 JSP基础语法总结

- JSP中直接编写普通字符串

- - 翻译到service方法的out.write("这里");

- <% %>

- - 翻译到service方法体内部，里面一条一条的java语句

- <%! %>

- - 翻译到service方法之外

- <% = %>

- - 翻译为service方法的out.print();

- <%@ page contentType ="text/html;charset=UTF-8"%>

- - page指令，通过contentType属性用来设置响应内容类型
  - charst指令，采用的字符集是UTF-8

- <%-- --%>

- - JSP专业注释



## 四、JSP和Servlet的本质区别（面试题）

> 职责不同

Servlet的职责是：收集数据

Servlet最强项的是逻辑处理，业务处理，然后连接数据库，获取/收集数据

JSP的职责是：展示数据

JSP最强项的是做数据的展示



## 五、JSP的page指令，解决响应时的中文乱码问题

:mag:配置指：`<%@page contentType="text/html;charset=UTF-8" %>`表示响应内容时text/html，采用的字符集是UTF-8

![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424182056.png)

相当于：

![image-20220424182518486](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424182519.png)



