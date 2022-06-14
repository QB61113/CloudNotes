# 整合SSM框架

页面访问流程：前端 ----> controller层 ----> service 层----> mapper层 ----> 数据库

前端访问，controller层到service层，service调mapper层，mapper连数据库

开发流程：数据库 ----> mapper层 ---> service业务层 ----> controller层 ----> 前端页面

# 1 开发

> **数据库环境**

创建一个数据库

```mysql
CREATE DATABASE `ssmbuild`;

USE `ssmbuild`;

DROP TABLE IF EXISTS `books`;

CREATE TABLE `books` (
`bookID` INT(10) NOT NULL AUTO_INCREMENT COMMENT '书id',
`bookName` VARCHAR(100) NOT NULL COMMENT '书名',
`bookCounts` INT(11) NOT NULL COMMENT '数量',
`detail` VARCHAR(200) NOT NULL COMMENT '描述',
KEY `bookID` (`bookID`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT  INTO `books`(`bookID`,`bookName`,`bookCounts`,`detail`)VALUES
(1,'Java',1,'从入门到放弃'),
(2,'MySQL',10,'从删库到跑路'),
(3,'Linux',5,'从进门到进牢');
```

​	

> **基本环境搭建**

1. 新建一个Maven项目，**添加Web支持**

2. 导入相关的pom依赖

   ```xml
   <!--依赖: junit,数据库驱动,连接池,servlet,jsp,mybatis,mybaits-spring,spring相关-->
       <dependencies>
           <!--Junit-->
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
           </dependency>
           <!--数据库驱动-->
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>5.1.47</version>
           </dependency>
   
           <!--Servlet - JSP -->
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>servlet-api</artifactId>
               <version>2.5</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet.jsp</groupId>
               <artifactId>jsp-api</artifactId>
               <version>2.2</version>
           </dependency>
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>jstl</artifactId>
               <version>1.2</version>
           </dependency>
   
           <!--Mybatis-->
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis</artifactId>
               <version>3.5.2</version>
           </dependency>
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis-spring</artifactId>
               <version>2.0.2</version>
           </dependency>
   
           <!--Spring-->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>5.1.9.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-jdbc</artifactId>
               <version>5.1.9.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <version>1.18.22</version>
           </dependency>
       </dependencies>
   
       <!--静态资源导出-->
       <build>
           <resources>
               <resource>
                   <directory>src/main/java</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                   </includes>
                   <filtering>false</filtering>
               </resource>
               <resource>
                   <directory>src/main/resources</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                   </includes>
                   <filtering>false</filtering>
               </resource>
           </resources>
       </build>
   ```

3. Maven静态资源过滤设置

   ```xml
   <build>
      <resources>
          <resource>
              <directory>src/main/java</directory>
              <includes>
                  <include>**/*.properties</include>
                  <include>**/*.xml</include>
              </includes>
              <filtering>false</filtering>
          </resource>
          <resource>
              <directory>src/main/resources</directory>
              <includes>
                  <include>**/*.properties</include>
                  <include>**/*.xml</include>
              </includes>
              <filtering>false</filtering>
          </resource>
      </resources>
   </build>
   ```

4. 可以在IDEA中连接数据库

   ![image-20220613153740118](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220613153740118.png)

5. 建立**基本的结构**和**配置框架**

   :mag:**java文件夹下**

   - com.xleixz.pojo（实体）

   - com.xleixz.mapper

   - com.xleixz.service（业务层）

   - com.xleixz.controller

   :mag:**resources文件夹下**

   - Mybatis-config.xml（Mybatis核心配置文件）

     ```xml
     <?xml version="1.0" encoding="UTF-8" ?>
     <!DOCTYPE configuration
             PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
             "http://mybatis.org/dtd/mybatis-3-config.dtd">
     <configuration>
     
     </configuration>
     ```

   - ApplicationContext.xml（Spring核心配置文件）

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd">
     
     </beans>
     ```

​	

> **Mybatis层编写**

1. 编写Mybatis核心配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
   	
       <!--类起别名-->
       <typeAliases>
           <package name="com.xleixz.pojo"/>
       </typeAliases>
       <!--绑定接口映射器-->
       <mappers>
           <mapper resource="com/xleixz/mapper/BookMapper.xml"/>
       </mappers>
   
   </configuration>
   ```

2. 编写数据库对应的实体类 【Books.java】

   **这里使用Lombok插件，使用注解**

   ```java
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class Books {
   
       private int bookID;
       private String bookName;
       private int bookCounts;
       private String detail;
   
   }
   ```

3. 编写mapper层的 Mapper接口

   ```java
   import com.xleixz.pojo.Books;
   import java.util.List;
   public interface BookMapper {
   
       //增加一个Book
       int addBook(Books book);
   
       //根据id删除一个Book
       int deleteBookById(int id);
   
       //更新Book
       int updateBook(Books books);
   
       //根据id查询,返回一个Book
       Books queryBookById(int id);
   
       //查询全部Book,返回list集合
       List<Books> queryAllBook();
   
   }
   ```

4. 编写接口对应的Mapper.xml 文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   
   <mapper namespace="com.xleixz.mapper.BookMapper">
   
       <!--增加一个Book-->
       <insert id="addBook" parameterType="Books">
           insert into ssmbuild.books(bookName,bookCounts,detail)
           values (#{bookName}, #{bookCounts}, #{detail})
       </insert>
   
       <!--根据id删除一个Book-->
       <delete id="deleteBookById" parameterType="int">
           delete from ssmbuild.books where bookID=#{bookID}
       </delete>
   
       <!--更新Book-->
       <update id="updateBook" parameterType="Books">
           update ssmbuild.books
           set bookName = #{bookName},bookCounts = #{bookCounts},detail = #{detail}
           where bookID = #{bookID}
       </update>
   
       <!--根据id查询,返回一个Book-->
       <select id="queryBookById" resultType="Books">
           select * from ssmbuild.books
           where bookID = #{bookID}
       </select>
   
       <!--查询全部Book-->
       <select id="queryAllBook" resultType="Books">
           SELECT * from ssmbuild.books
       </select>
   
   </mapper>
   ```

   <font color="red">**写完映射文件之后，要立马绑定到Mybatis核心配置文件中！！！**</font>

   ```xml
   <mappers>
           <mapper resource="com/xleixz/mapper/BookMapper.xml"/>
       </mappers>
   ```

5. 编写Service层的接口和实现类

   <font color="red">**业务层（Service）接口和mapper层接口本质上没有区别！！**</font>

   <font color="orange">**逻辑：Service业务层调mapper层，组合mapper层，私有化`  private BookMapper bookMapper;`**</font>

   **Service业务层接口**

   ```java
   public interface BookService {
   
       //增加一个Book
       int addBook(Books book);
   
       //根据id删除一个Book
       int deleteBookById(int id);
   
       //更新Book
       int updateBook(Books books);
   
       //根据id查询,返回一个Book
       Books queryBookById(int id);
   
       //查询全部Book,返回list集合
       List<Books> queryAllBook();
   
   }
   ```

   **Service业务层实现类**

   ```java
   import com.xleixz.mapper.BookMapper;
   import com.xleixz.pojo.Books;
   
   import java.util.List;
   
   public class BookServiceImpl implements BookService {
   
       //调用mapper层的操作，设置一个set接口，方便Spring管理
       private BookMapper bookMapper;
   
       public void setBookMapper(BookMapper bookMapper) {
           this.bookMapper = bookMapper;
       }
   
       @Override
       public int addBook(Books book) {
           return bookMapper.addBook(book);
       }
   
       @Override
       public int deleteBookById(int id) {
           return bookMapper.deleteBookById(id);
       }
   
       @Override
       public int updateBook(Books books) {
           return bookMapper.updateBook(books);
       }
   
       @Override
       public Books queryBookById(int id) {
           return bookMapper.queryBookById(id);
       }
   
       @Override
       public List<Books> queryAllBook() {
           return bookMapper.queryAllBook();
       }
   }
   ```

<font color="green">**到这里MVC的底层Mybatis层就编写完了！**</font>

<font color="green">**pojo：实体类         mapper层和service层就是MVC的M（Model模型）**</font>

​	

> **Spring层**

1. 配置**Spring整合MyBatis**，这里数据源使用jdbc连接池；

2. 准备Spring整合Mybatis的相关的配置文件【Spring-mapper.xml】

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd">
       
   </beans>
   ```

   编写配置文件【Spring-Mapper.xml】，<font color="red">**如果使用的是MySQL8.0+，要增加一个时区的配置；**</font>

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd">
   
       <!-- 配置整合mybatis -->
       <!-- 1.数据库连接池 -->
       <!--DataSource:使用Spring的数据源替换mybatis的配置  jdbc c3p0  dbcp
       这里使用Spring提供的jdbc-->
       <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
           <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
           <property name="url"
                     value="jdbc:mysql://localhost:3306/ssmbuild?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF8"/>
           <property name="username" value="root"/>
           <property name="password" value="123456"/>
       </bean>
   
       <!-- 2.配置SqlSessionFactory对象 -->
       <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
           <!-- 注入数据库连接池 -->
           <property name="dataSource" ref="dataSource"/>
           <!-- 配置Mybatis全局配置文件:Mybatis-config.xml -->
           <property name="configLocation" value="classpath:Mybatis-config.xml"/>
       </bean>
   
       <!-- 3.配置扫描Mapper接口包，动态实现Mapper接口注入到spring容器中 -->
       <!--解释 ：https://www.cnblogs.com/jpfss/p/7799806.html-->
       <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
           <!-- 注入sqlSessionFactory -->
           <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
           <!-- 给出需要扫描Mapper接口包 -->
           <property name="basePackage" value="com.xleixz.mapper"/>
       </bean>
   
   </beans>
   ```
   
3. Spring整合service层

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context.xsd">
   
       <!-- 扫描service相关的bean -->
       <context:component-scan base-package="com.xleixz.service" />
   
       <!--BookServiceImpl注入到IOC容器中-->
       <bean id="BookServiceImpl" class="com.xleixz.service.BookServiceImpl">
           <property name="bookMapper" ref="bookMapper"/>
       </bean>
   
       <!-- 配置事务管理器 -->
       <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <!-- 注入数据库连接池 -->
           <property name="dataSource" ref="dataSource" />
       </bean>
   
   </beans>
   ```

**Spring层编写完成！**

​	

> **SpringMVC层**

1. 增加Web支持，配置web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
            version="5.0">
   
       <!--DispatcherServlet-->
       <servlet>
           <servlet-name>DispatcherServlet</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <!--一定要注意:我们这里加载的是总的配置文件，之前被这里坑了！-->
               <param-value>classpath:ApplicationContext.xml</param-value>
           </init-param>
           <load-on-startup>1</load-on-startup>
       </servlet>
       <servlet-mapping>
           <servlet-name>DispatcherServlet</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   
       <!--encodingFilter-->
       <filter>
           <filter-name>encodingFilter</filter-name>
           <filter-class>
               org.springframework.web.filter.CharacterEncodingFilter
           </filter-class>
           <init-param>
               <param-name>encoding</param-name>
               <param-value>utf-8</param-value>
           </init-param>
       </filter>
       <filter-mapping>
           <filter-name>encodingFilter</filter-name>
           <url-pattern>/*</url-pattern>
       </filter-mapping>
   
       <!--Session过期时间-->
       <session-config>
           <session-timeout>15</session-timeout>
       </session-config>
   </web-app>
   ```

2. 编写**Spring-mvc.xml**配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context.xsd
      http://www.springframework.org/schema/mvc
      https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
       <!-- 配置SpringMVC -->
       <!-- 1.开启SpringMVC注解驱动 -->
       <mvc:annotation-driven />
       <!-- 2.静态资源默认servlet配置-->
       <mvc:default-servlet-handler/>
   
       <!-- 3.配置jsp 显示ViewResolver视图解析器 -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <property name="viewClass" value="org.springframework.web.servlet.view.JstlView" />
           <property name="prefix" value="/" />
           <property name="suffix" value=".jsp" />
       </bean>
   
       <!-- 4.扫描web相关的bean -->
       <context:component-scan base-package="com.xleixz.controller" />
   
   </beans>
   ```

3. **Spring配置整合文件，applicationContext.xml**

   ```xml
   <import resource="Spring-mapper.xml"/>
   <import resource="Spring-service.xml"/>
   <import resource="Spring-mvc.xml"/>
   ```

<font color="green">**到这里所有的配置文件就全部结束了。**</font>

**框架配置结构：**

![image-20220613174819509](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220613174819509.png)

​	

1. BookController 类编写

   ```java
   import com.xleixz.pojo.Books;
   import com.xleixz.service.BookService;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.beans.factory.annotation.Qualifier;
   import org.springframework.stereotype.Controller;
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.PathVariable;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   import java.util.List;
   
   @Controller
   @RequestMapping("/book")
   public class BookController {
   
       @Autowired
       @Qualifier("BookServiceImpl")
       private BookService bookService;
   
       @RequestMapping("/allBook")
       public String list(Model model) {
           List<Books> list = bookService.queryAllBook();
           model.addAttribute("list", list);
           return "allBook";
       }
   
       @RequestMapping("/toAddBook")
       public String toAddPaper() {
           return "addBook";
       }
   
       @RequestMapping("/addBook")
       public String addPaper(Books books) {
           System.out.println(books);
           bookService.addBook(books);
           return "redirect:/book/allBook";
       }
   
       @RequestMapping("/toUpdateBook")
       public String toUpdateBook(Model model, int id) {
           Books books = bookService.queryBookById(id);
           System.out.println(books);
           model.addAttribute("book",books );
           return "updateBook";
       }
   
       @RequestMapping("/updateBook")
       public String updateBook(Model model, Books book) {
           System.out.println(book);
           bookService.updateBook(book);
           Books books = bookService.queryBookById(book.getBookID());
           model.addAttribute("books", books);
           return "redirect:/book/allBook";
       }
   
       @RequestMapping("/del/{bookId}")
       public String deleteBook(@PathVariable("bookId") int id) {
           bookService.deleteBookById(id);
           return "redirect:/book/allBook";
       }
   
   }
   ```

2. 补充index.jsp页面

   ```jsp
   <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
   <!DOCTYPE HTML>
   <html>
   <head>
     <title>首页</title>
     <style type="text/css">
       a {
         text-decoration: none;
         color: black;
         font-size: 18px;
       }
       h3 {
         width: 180px;
         height: 38px;
         margin: 100px auto;
         text-align: center;
         line-height: 38px;
         background: deepskyblue;
         border-radius: 4px;
       }
     </style>
   </head>
   <body>
   
   <h3>
     <a href="<%=request.getContextPath()%>/book/allBook">点击进入列表页</a>
   </h3>
   </body>
   </html>
   ```

3. 书籍列表页面 **allbook.jsp**

   ```jsp
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
      <title>书籍列表</title>
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <!-- 引入 Bootstrap -->
      <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
   </head>
   <body>
   
   <div class="container">
   
      <div class="row clearfix">
          <div class="col-md-12 column">
              <div class="page-header">
                  <h1>
                      <small>书籍列表 —— 显示所有书籍</small>
                  </h1>
              </div>
          </div>
      </div>
   
      <div class="row">
          <div class="col-md-4 column">
              <a class="btn btn-primary" href="${pageContext.request.contextPath}/book/toAddBook">新增</a>
          </div>
      </div>
   
      <div class="row clearfix">
          <div class="col-md-12 column">
              <table class="table table-hover table-striped">
                  <thead>
                  <tr>
                      <th>书籍编号</th>
                      <th>书籍名字</th>
                      <th>书籍数量</th>
                      <th>书籍详情</th>
                      <th>操作</th>
                  </tr>
                  </thead>
   
                  <tbody>
                  <c:forEach var="book" items="${requestScope.get('list')}">
                      <tr>
                          <td>${book.getBookID()}</td>
                          <td>${book.getBookName()}</td>
                          <td>${book.getBookCounts()}</td>
                          <td>${book.getDetail()}</td>
                          <td>
                              <a href="${pageContext.request.contextPath}/book/toUpdateBook?id=${book.getBookID()}">更改</a> |
                              <a href="${pageContext.request.contextPath}/book/del/${book.getBookID()}">删除</a>
                          </td>
                      </tr>
                  </c:forEach>
                  </tbody>
              </table>
          </div>
      </div>
   </div>
   ```
   
4. 添加书籍页面：**addBook.jsp**

   ```jsp
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   
   <html>
   <head>
     <title>新增书籍</title>
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <!-- 引入 Bootstrap -->
     <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
   </head>
   <body>
   <div class="container">
   
     <div class="row clearfix">
       <div class="col-md-12 column">
         <div class="page-header">
           <h1>
             <small>新增书籍</small>
           </h1>
         </div>
       </div>
     </div>
     <form action="${pageContext.request.contextPath}/book/addBook" method="post">
       书籍名称：<input type="text" name="bookName"><br><br><br>
       书籍数量：<input type="text" name="bookCounts"><br><br><br>
       书籍详情：<input type="text" name="detail"><br><br><br>
       <input type="submit" value="添加">
     </form>
   
   </div>
   ```

5. 修改书籍页面  **updateBook.jsp**

   ```jsp
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
     <title>修改信息</title>
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <!-- 引入 Bootstrap -->
     <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
   </head>
   <body>
   <div class="container">
   
     <div class="row clearfix">
       <div class="col-md-12 column">
         <div class="page-header">
           <h1>
             <small>修改信息</small>
           </h1>
         </div>
       </div>
     </div>
   
     <form action="${pageContext.request.contextPath}/book/updateBook" method="post">
       <input type="hidden" name="bookID" value="${book.getBookID()}"/>
       书籍名称：<input type="text" name="bookName" value="${book.getBookName()}"/>
       书籍数量：<input type="text" name="bookCounts" value="${book.getBookCounts()}"/>
       书籍详情：<input type="text" name="detail" value="${book.getDetail() }"/>
       <input type="submit" value="提交"/>
     </form>
   
   </div>
   ```

​	

> **拓展：新增一个查询功能**



​	

目录结构：

![image-20220614112019556](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220614112019556.png)

---

​	

# 2 报错问题

> <font color="red">**启动测试，发现报500服务器内部错误！**</font>

![image-20220613183340511](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220613183340511.png)

<font color="green">**解决：在web.xml配置文件中，classpath：对应的文件中没有绑定ApplicationContext.xml配置文件，修改一下。**</font>

![image-20220613183557168](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220613183557168.png)

​	

> <font color="red">**新增书籍出现问题：数据库无法正常添加数据**</font>

<font color="orange">**分析：**</font>

1. 首先判断事务是否提交，若没有则配置事务；若还有问题则下一步；
2. 看一下SQL语句能否执行成功，SQL执行失败，修改未完成。

<font color="green">**解决：**</font>

<font color="green">**SQL语句中没有传入id，service业务层的BookID没有传过来，设置前端传递隐藏域**</font>

```java
<input type="hidden" name="bookID" value="${book.getBookID()}"/>
```

这里`name`要对应数据库里的**列**。

**还可以通过在mybatis核心配置文件中添加日志，查看SQL语句是否正常**

```xml
<settings>
    <setting name="logImpl" values="STDOUT_LOGGING"/>
</settings>
```



