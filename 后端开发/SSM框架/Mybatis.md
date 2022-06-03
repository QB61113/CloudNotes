# Mybatis

<hr>

# 1、Mybatis简介

## 1.1 什么是Mybatis

**什么是Mybatis：**Mybatis是一款优秀的**持久层框架**，它支持定制化SQL，存储过程以及高级映射（方便写SQL）。
**Mybatis**避免了几乎所有的JDBC代码和手动设置参数以及获取结果集。
**Mybatis**可以使用简单的xml或注解来配置和映射原生类型、接口和Java的POJO（Plain Old Objects，普通老式Java对象）为数据库中的记录。

**Mybatis**本是Apache的一个开源项目，2013年迁移到GitHub。

## 1.2 获取Mybatis

**Maven仓库：**[https://mvnrepository.com/](https://mvnrepository.com/)

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.7</version>
</dependency>

```

​	

![20220424133612](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424133612.png)

​	

**GitHub仓库：**[https://github.com/](https://github.com/)

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424133659.png" alt="20220424133659" style="zoom:50%;" />

​	



![20220424133716](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424133716.png)

​	

**中文帮助文档：**[https://github.com/tuguangquan/mybatis](https://github.com/tuguangquan/mybatis)

## 1.3 持久化与持久层

数据持久化
**持久化**就是将程序的数据在持久状态和瞬时状态转化的过程。
内存的特性：**断电即失。**

​	

**为什么需要持久化？**
有一些对象，不能让他丢掉。内存太贵。

​	

**持久层**：完成持久化工作的代码块。**层**是界限十分明显的。

## 1.4 为什么需要Mybatis？

帮助程序员将数据存入数据库中。
特性：方便。传统的JDBC代码太复杂。简化，有现成的框架。自动化操作。
**使用的人多！！！！**

---

​		

# 2、第一个Mybatis程序

思路：搭建环境---->导入Mybatis---->编写代码---->测试

> 帮助文档传送门：[Mybatis帮助文档](https://mybatis.org/mybatis-3/zh/index.html)

## 2.1 搭建环境

搭建数据库

```sql
Create database mybatis;
use mybatis;

create table user(
						id int(20) not null PRIMARY key,
						name VARCHAR(255) not null,
						pwd VARCHAR(255) not null
					
)ENGINE=INNODB DEFAULT CHARSET=utf8;


INSERT into user(id,name,pwd) VALUES
(1,'张三','123456'),
(2,'李四','123456'),
(3,'王五','123456');
```

​	

## 2.2 创建项目

创建一个普通的Maven项目，及时将path文件修改为正确的目录，避免占用C盘资源。
<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424133851.png" alt="20220424133851" style="zoom:50%;" />

​	

删除src目录，可以当做父工程使用。
![20220424133857](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424133857.png)

​	

 在pom.xml文件中导入依赖

```xml
<!--导入依赖-->
    <dependencies>
        <!--mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <!--mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <!--junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>
```

​	

## 2.3 创建模块

new---->Module---->Maven，在resources里新建一个xml配置文件
![20220424133907](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424133907.png)

​	

1. 编写Mybatis工具类

理解：
`String resource = "org/mybatis/example/mybatis-config.xml";`读取配置文件
然后封装成一个工具类。
`InputStream inputStream = Resources.getResourceAsStream(resource); `
`SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);`
![20220424133916](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424133916.png)

Mybatis工具类：
<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424133925.png" alt="20220424133925" style="zoom:50%;" />

```java
public class MybatisUtils {

    private static SqlSessionFactory sqlSessionFactory;

    static {


        try {
            //使用Mybatis第一步，必须要做！！！
            //获取SqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    //既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例。
    //org.apache.ibatis.session.SqlSession 提供了在数据库执行 SQL 命令所需的所有方法

    public static SqlSession getSqlSession() {

        return sqlSessionFactory.openSession();
    }
}
```

​	

2. 编写Mybatis的核心配置文件。

Mybatis-config.xml（注意这里[测试常见报错）](#abN9C)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!--configuration核心配置文件-->
<configuration>
    <environments default="development">
        <environment id="development">
            <!--transactionManager事务管理-->
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <!--防止出现问号-->
                <property name="url"
                          value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF8"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
  
  <!--这里需要配置文件，见5.1-->
</configuration>
```

​	

## 2.4 IDEA连接数据库（可选）

> 为了方便显示数据库内的数据，可在IDEA中连接数据库。

1. 点击`Database`，点击`+`号，添加**Data Source**中的`MySQL`，

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220504181711.png" alt="20220504181711" style="zoom:50%;" />

2. 在**General**中输入`User`（账号）、`Password`（密码），点击`Test Connection`测试连接，成功后点击`Schemas`。

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220504181914.png" alt="20220504181914" style="zoom:50%;" />

3. 在`Schemas`中勾选需要用到的数据库，点击OK即可。

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220504182107.png" alt="20220504182107" style="zoom: 50%;" />

​		

## 2.5 编写代码

1. **实体类**

文件：User
位置：
![20220424134002](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424134002.png)

```java
//实体类

public class User {
    private int id;
    private String name;
    private String pwd;

    public User() {
    }

    public User(int id, String name, String pwd) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }
}


```

​	

2. **Mapper接口**

文件：UserDao（Dao等价于Mapper。）
位置：
![20220430002239](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220430002239.png)

```java
//Dao等价于Mapper

public interface UserDao {
    List<User> getUserList();
}
```

​	

3. **接口实现类**

由原来的UserDaoImpl转变为一个Mapper配置文件
文件：UserMapper.xml
位置：
![20220424134044](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424134044.png)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace绑定一个对应的Dao/Mapper接口-->
<mapper namespace="xleixz.dao.UserDao">
    
    <!--select查询语句-->
    <select id="getUserList" resultType="xleixz.pojo.User">
        select * from mybatis.user
    </select>

</mapper>
```

​	

## 2.6 测试

1. **Junit测试**

> 方式一（推荐）

```java
public class UserDaoTest<userList> {

    @Test
    public void test() {

        //第一步：获取SqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        //第二步：执行SQL
        //方式一：
        UserDao userDao = sqlSession.getMapper(UserDao.class);
        List<User> userList = userDao.getUserList();

        for (User user : userList) {
            System.out.println(user);
        }

        //关闭SqlSession
        sqlSession.close();
    }
}

```

![20220424134107](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424134107.png)

​	

> 方式二（较老，不推荐，了解）

```java
package xleixz.dao;

import org.apache.ibatis.session.SqlSession;
import org.junit.Test;
import xleixz.pojo.User;
import xleixz.utils.MybatisUtils;

import java.util.List;

/**
 * @Author: 小雷学长
 * @Date: 2022/4/17 - 16:36
 * @Version: 1.8
 */

public class UserDaoTest<userList> {

    @Test
    public void test() {

        //第一步：获取SqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        //第二步：执行SQL
        //方式一：
        /*UserDao userDao = sqlSession.getMapper(UserDao.class);
        List<User> userList = userDao.getUserList();*/

        //方式二：
        List<User> userList =  sqlSession.selectList("xleixz.dao.UserDao.getUserList");


        for (User user : userList) {
            System.out.println(user);
        }

        //关闭SqlSession
        sqlSession.close();
    }
}

```

​	

 ## 2.7 错误分析

1. **常见报错类型一**

`org.apache.ibatis.binding.BindingException: Type interface xleixz.dao.UserMapper is not known to the MapperRegistry.`
**MapperRegistry**是什么？如何解决？

> ---->解决：在[编写Mybatis的核心配置文件](#Yph05)中的xml添加注册mappers配置。

```xml
<!--每一个Mapper.xml都需要在Mybatis核心配置文件中注册！！-->
    <mappers>
        <mapper resource="xleixz/dao/UserMapper.xml"/>
    </mappers>
```

​	

2. **常见报错类型二**

`The error may exist in xleixz/dao/UserMapper.xml`
<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424134130.png" alt="20220424134130" style="zoom:50%;" />
原因是Test中没有**UserMapper.xml**文件，可以通过手动复制到Test中，但是这样**太多！太麻烦！**

> ---->解决：手动配置资源过滤，将Build配置信息导入到**pop**文件中。

```xml
<build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
        </resources>
    </build>
```

3. **报错类型三**

`Sun Apr 17 18:17:14 CST 2022 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.`

> ---->解决：将配置文件里的**useSSL=true**改成**useSSL=false**

```xml
value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF8"/>
```

---

​	

​	

# 3、Mybatis增删改查实现

==重点：==**增删改**需要***提交事务***

## 3.1 namespace

==namespace==中的包名要和Mapper接口的包名一致
<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424134307.png" alt="20220424134307" style="zoom:50%;" />

​	

## 3.2 select

选择，查询语句。
**id**：就是对应的namespace中的方法名；
**resultType**：SQL语句执行的返回值；
**parameterType**：参数类型；

### 3.2.1 步骤详解

1. 编写接口；

```java
public interface UserMapper {
      //根据ID查询用户
    User getUserById(int id);
}
```

2. 编写对应的Mapper中对的SQL语句；

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace绑定一个对应的Dao/Mapper接口-->
<mapper namespace="xleixz.dao.UserMapper">
  
    <select id="getUserById" parameterType="int" resultType="xleixz.pojo.User">
        select *
        from mybatis.user
        where id = #{id}
    </select>
  </mapper>
```

3. 测试。(**增删改需要提交事务**）

```java
 //增删改需要提交事务
    //测试插入
    @Test
    public void addUser() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        mapper.addUser(new User(4, "小雷", "123456"));

        //提交事务
        sqlSession.commit();

        //关闭SqlSession
        sqlSession.close();

    }
```

​	

## 3.3 insert

注意：**增删改需要提交事务**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace绑定一个对应的Dao/Mapper接口-->
<mapper namespace="xleixz.dao.UserMapper">
  
    <!--对象中的属性可以直接取出来-->
    <insert id="addUser" parameterType="xleixz.pojo.User">
        insert into mybatis.user(id, name, pwd)
        values (#{id}, #{name}, #{pwd})
    </insert>
  </mapper>
```

​	

## 3.4 update

注意：**增删改需要提交事务**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace绑定一个对应的Dao/Mapper接口-->
<mapper namespace="xleixz.dao.UserMapper">
  
    <update id="updateUser" parameterType="xleixz.pojo.User">
        update mybatis.user
        set name = #{name},
            pwd  = #{pwd}
        where id = #{id}
    </update>
  </mapper>
```

​	

## 3.5 delete

注意：**增删改需要提交事务**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace绑定一个对应的Dao/Mapper接口-->
<mapper namespace="xleixz.dao.UserMapper">
  
<delete id="deleteUser" parameterType="int">
        delete from mybatis.user
        where id = #{id}
    </delete>
  </mapper>
```

​	

## 3.6 错误分析

**标签**不能匹配错，标签必须相对应，插入就插入，查询就查询。

resources文件夹下的xml中，`mappers`标签中的**resource**值中的不是`.`，而是`/`符号。

```xml
<!--每一个Mapper.xml都需要在Mybatis核心配置文件中注册！！-->
    <mappers>
        <mapper resource="xleixz/dao/UserMapper.xml"/>
    </mappers>
```

程序配置文件，必须符合规则！！

![20220424134332](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424134332.png)
`NullPointerException`**空指针异常**报错，原因是没有注册到资源！！

**输出的xml文件中存在中文乱码问题！**

**Maven资源无法导出问题**，解决---->在Maven中加入`build`，详情见[Maven资源无法导出](https://www.yuque.com/xleixz/ksbdf5/cu8qnq#Fus64)

---

​	

# 4、Map的使用

> 假设，实体类或数据库中的表，字段或者参数过多，应当考虑使用**Map**！

```java
   //万能的Map
    int addUser2(Map<String, Object> map);
```

```xml
  <!--对象中的属性可以直接取出来
    传递Map的key-->
    <insert id="addUser2" parameterType="map">
        insert into mybatis.user(id, name, pwd)
        values (#{userid}, #{username}, #{password})
    </insert>
```

```java
@Test
    public void addUser2() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        Map<String, Object> map = new HashMap<String,Object>();
        map.put("userid",5);
        map.put("username","Hello");
        map.put("password",222333);

        mapper.addUser2(map);

        //提交事务
        sqlSession.commit();

        //关闭SqlSession
        sqlSession.close();

    }
```

**Map**传递参数，直接在SQL中取出key即可！`parameterType="map"`
**对象**传递参数，直接在SQL中取出对象的属性即可！`parameterType="Object"`
只有 **一个基本类型**参数的情况，直接在SQL中取到！  **省略不写**

**多个参数用Map！，或注解！**

----

​		

# 5、模糊查询拓展

**模糊查询怎么写？**
传送门：[MySQL的模糊查询](https://www.yuque.com/go/doc/60949063)
在Java代码查询执行的时候，传递通配符`% %`

接口类

```java
public interface UserMapper {

    //模糊查询一个用户
    List<User> getUserLike(String value);
}
```

接口实现类

```xml
 <!--模糊查询用户信息-->
    <select id="getUserLike" resultType="xleixz.pojo.User">
        select *
        from mybatis.user
        where name like #{value }
    </select>
```

测试类

```java
 @Test
    public void getUserLike() {

        SqlSession sqlSession = MybatisUtils.getSqlSession();

        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        List<User> userList = mapper.getUserLike("%李%");

        for (User user : userList) {
            System.out.println(user);
        }

        //关闭SqlSession
        sqlSession.close();
    }
```

![20220424134352](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424134352.png)

---

​	

# 6、XML配置解析

## 6.1 属性优化

### 6.1.1 核心配置文件

核心配置文件为：**mybatis.config.xml**
Mybatis的配置文件包含了会深深影响Mybatis行为的设置和属性信息。

[**XML配置**](https://mybatis.org/mybatis-3/zh/configuration.html)**（官网帮助文档）**
 configuration（配置） 

- [properties（属性）](https://mybatis.org/mybatis-3/zh/configuration.html#properties)
- [settings（设置）](https://mybatis.org/mybatis-3/zh/configuration.html#settings)
- [typeAliases（类型别名）](https://mybatis.org/mybatis-3/zh/configuration.html#typeAliases)
- [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
- [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
- [plugins（插件）](https://mybatis.org/mybatis-3/zh/configuration.html#plugins)
- [environments（环境配置）](https://mybatis.org/mybatis-3/zh/configuration.html#environments)
  -  environment（环境变量） 
     - transactionManager（事务管理器）
     - dataSource（数据源）
- [databaseIdProvider（数据库厂商标识）](https://mybatis.org/mybatis-3/zh/configuration.html#databaseIdProvider)
- [mappers（映射器）](https://mybatis.org/mybatis-3/zh/configuration.html#mappers)

​	

### 6.1.2 环境配置（environments）

**MyBatis 可以配置成适应多种环境**

- [x] 理解：

在`environments`标签中可以配置多个`environment`环境，但是默认是**environments default=**后的id。
若想更换默认环境，则修改**default**的id值即可。

```xml
<!--configuration核心配置文件-->
<configuration>
  <!--default默认的id就是默认的配置-->
    <environments default="development-01">
        <environment id="development-01">
           <!--这里是环境development-01-->
        </environment>
      <environment id="development-02">
           <!--这里是环境development-02-->
        </environment>
    </environments>
```

**不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。**

​	

### 6.1.3 事务管理器（transactionManager）

==了解即可==

在 MyBatis 中有两种类型的事务管理器（也就是 type="[JDBC|MANAGED]"）：

-  **JDBC** – 这个配置直接使用了 JDBC 的提交和回滚设施，它依赖从数据源获得的连接来管理事务作用域。 
-  **MANAGED** – 这个配置几乎没做什么。它从不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（比如 JEE 应用服务器的上下文）。 默认情况下它会关闭连接。然而一些容器并不希望连接被关闭，因此需要将 closeConnection 属性设置为 false 来阻止默认的关闭行为。

​	

### 6.1.4 数据源（dataSource）

==了解即可==
**池**：用完可以回收。
dataSource 元素使用标准的 JDBC 数据源接口来配置 JDBC 连接对象的资源。
大多数 MyBatis 应用程序会按示例中的例子来配置数据源。虽然数据源配置是可选的，但如果要启用延迟加载特性，就必须配置数据源。 

有三种内建的数据源类型（也就是 type="[UNPOOLED|POOLED|JNDI]"）：
**UNPOOLED **– 这个数据源的实现会每次请求时打开和关闭连接。虽然有点慢，但对那些数据库连接可用性要求不高的简单应用程序来说，是一个很好的选择。 性能表现则依赖于使用的数据库，对某些数据库来说，使用连接池并不重要，这个配置就很适合这种情形。
**POOLED** – 这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来，避免了创建新的连接实例时所必需的初始化和认证时间。 这种处理方式很流行，能使并发 Web 应用快速响应请求。  

**JNDI** – 这个数据源实现是为了能在如 EJB 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的数据源引用。  

​	

**重点**
**Mybatis默认的事物管理器就是JDBC，连接池就是POOLED。**

​	

### 6.1.5 属性（properties）

我们可以通过properties属性来实现引用配置文件。

这些属性可以在外部进行配置，并可以进行动态替换。你既可以在典型的 Java 属性文件中配置这些属性，也可以在 properties 元素的子元素中设置。  

编写一个配置文件
db.properties

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF8
username=root
password=123456
```

在**核心配置文件**mybatis-config.xml中引入**properties**
注意：在xml中，所有的标签都可以规定其顺序，**properties**只能放在最上面

```xml
<!--引入外部配置文件-->
    <properties resource="db.properties">
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
    </properties>
```

注意：可以直接引入外部文件，也可以在其中增加一些属性配置，如果两个文件有同一字段，**优先使用外部配置文件！**
**优先使用外部配置文件**理解：当配置文件**db.properties**中写了username和password，**核心配置文件**mybatis-config.xml中的`properties`标签中也写了username和password，则优先使用**db.propertise**文件中的配置信息。

​	

## 6.2 别名优化

### 6.2.1 类型别名（typeAliases）

 类型别名可为 Java 类型设置一个缩写名字。 它仅用于 XML 配置，意在降低冗余的全限定类名书写。  

**方式一：**

原来是这样：注意resultType：

```xml
<mapper namespace="xleixz.dao.UserMapper">
    <!--select查询语句-->
    <select id="getUserList" resultType="xleixz.pojo.User">
        select *
        from mybatis.user;
    </select>
</mapper>
```

在**核心配置文件mybatis-config.xml**中添加别名配置。

```xml
 <!--可以给实体类起别名-->
    <typeAliases>
        <typeAlias type="xleixz.pojo.User" alias="User"/><!--alias起别名-->
    </typeAliases>
```

这样接口实现类中**resultType值**可优化为：

```xml
<mapper namespace="xleixz.dao.UserMapper">
    <!--select查询语句-->
    <select id="getUserList" resultType="User">
        select *
        from mybatis.user;
    </select>
  </mapper>
```

​	

**方式二：**

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean ，比如：
扫描实体类的包，它的默认别名就为这个类的类名，**首字母小写！**

在**核心配置文件mybatis-config.xml**中添加别名配置。

```xml
<!--可以给实体类起别名-->
    <typeAliases>
        <package name="xleixz.pojo"/>
    </typeAliases>
```

这样接口实现类中**resultType值**可优化为：（建议小写）

```xml
 <!--select查询语句-->
    <select id="getUserList" resultType="user">
        select *
        from mybatis.user;
    </select>
```

​	

**方法三：**

若有注解，则别名为其注解值。  

在实体类上方添加注解

```java
@Alias("user")
public class User {
}
```

​	

**方法总结和区别**：

如果实体类很多，建议使用**方式二；**
**方式一**可以DIY别名，第二种则不行。

下面是一些为常见的 Java 类型内建的类型别名。它们都是不区分大小写的，注意，为了应对原始类型的命名重复，采取了特殊的命名风格。

| 别名       | 映射的类型 |
| ---------- | ---------- |
| _byte      | byte       |
| _long      | long       |
| _short     | short      |
| _int       | int        |
| _integer   | int        |
| _double    | double     |
| _float     | float      |
| _boolean   | boolean    |
| string     | String     |
| byte       | Byte       |
| long       | Long       |
| short      | Short      |
| int        | Integer    |
| integer    | Integer    |
| double     | Double     |
| float      | Float      |
| boolean    | Boolean    |
| date       | Date       |
| decimal    | BigDecimal |
| bigdecimal | BigDecimal |
| object     | Object     |
| map        | Map        |
| hashmap    | HashMap    |
| list       | List       |
| arraylist  | ArrayList  |
| collection | Collection |
| iterator   | Iterator   |

​	

### 6.2.2设置（settings）

 这是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为。 下表描述了设置中各项设置的含义、默认值等。  

| 设置名                           | 描述                                                         | 有效值              | 默认值                                       |
| -------------------------------- | ------------------------------------------------------------ | ------------------- | -------------------------------------------- |
| cacheEnabled                     | 全局性地开启或关闭所有映射器配置文件中已配置的任何缓存。     | true &#124; false   | true                                         |
| lazyLoadingEnabled               | 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置 fetchType 属性来覆盖该项的开关状态。 | true &#124; false   | false                                        |
| aggressiveLazyLoading            | 开启时，任一方法的调用都会加载该对象的所有延迟加载属性。 否则，每个延迟加载属性会按需加载（参考 lazyLoadTriggerMethods)。 | true &#124; false   | false （在 3.4.1 及之前的版本中默认为 true） |
| multipleResultSetsEnabled        | 是否允许单个语句返回多结果集（需要数据库驱动支持）。         | true &#124; false   | true                                         |
| useColumnLabel                   | 使用列标签代替列名。实际表现依赖于数据库驱动，具体可参考数据库驱动的相关文档，或通过对比测试来观察。 | true &#124; false   | true                                         |
| useGeneratedKeys                 | 允许 JDBC 支持自动生成主键，需要数据库驱动支持。如果设置为 true，将强制使用自动生成主键。尽管一些数据库驱动不支持此特性，但仍可正常工作（如 Derby）。 | true &#124; false   | False                                        |
| autoMappingBehavior              | 指定 MyBatis 应如何自动映射列到字段或属性。 NONE 表示关闭自动映射；PARTIAL 只会自动映射没有定义嵌套结果映射的字段。 FULL 会自动映射任何复杂的结果集（无论是否嵌套）。 | NONE, PARTIAL, FULL | PARTIAL                                      |
| autoMappingUnknownColumnBehavior | 指定发现自动映射目标未知列（或未知属性类型）的行为。         |                     |                                              |

 一个配置完整的 settings 元素的示例如下：  

```xml
<settings>
  <setting name="cacheEnabled" value="true"/>
  <setting name="lazyLoadingEnabled" value="true"/>
  <setting name="multipleResultSetsEnabled" value="true"/>
  <setting name="useColumnLabel" value="true"/>
  <setting name="useGeneratedKeys" value="false"/>
  <setting name="autoMappingBehavior" value="PARTIAL"/>
  <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
  <setting name="defaultExecutorType" value="SIMPLE"/>
  <setting name="defaultStatementTimeout" value="25"/>
  <setting name="defaultFetchSize" value="100"/>
  <setting name="safeRowBoundsEnabled" value="false"/>
  <setting name="mapUnderscoreToCamelCase" value="false"/>
  <setting name="localCacheScope" value="SESSION"/>
  <setting name="jdbcTypeForNull" value="OTHER"/>
  <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
</settings>
```

​	

## 6.3 其他配置【了解即可】

 MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。 配置文档的顶层结构如下：  

了解即可

- [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
- [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
- [plugins（插件）](https://mybatis.org/mybatis-3/zh/configuration.html#plugins)
  - mybatis-generator-core
  - mybatis-plus
  - 通用Mapper

​	

## 6.4 映射器（mappers）

MapperRegistry：注册绑定我们的Mapper文件。

**方式一**==【推荐使用】==

推荐使用resource方式：

```xml
<!--每一个Mapper.xml都需要在Mybatis核心配置文件中注册！！-->
    <mappers>
        <mapper resource="xleixz/dao/UserMapper.xml"/>
    </mappers>
```

​	

**方式二**

class方式，使用class文件绑定注册：

```xml
<!--每一个Mapper.xml都需要在Mybatis核心配置文件中注册！！-->
  <mappers>
        <mapper class="xleixz.dao.UserMapper"/>
  </mappers>
```

问题注意点：

- 接口和它的Mapper配置文件必须同名！
- 接口和它的Mapper配置文件必须在同一包下！

​	

**方式三**

package方式，使用扫描包记性注入绑定：

```xml
<!--每一个Mapper.xml都需要在Mybatis核心配置文件中注册！！-->
<mappers>
        <package name="xleixz.dao"/>
</mappers>
```

---

​	

# 7、生命周期和作用域（Scope）

---

**生命周期**和**作用域**是至关重要的，因为错误的使用会导致非常严重的**并发问题**。  

程序运行流程：
<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424134434.png" alt="20220424134434" style="zoom:50%;" />

**SqlSessionFactoryBuilder：**

- 一旦创建了SqlSessionFactory，就不在需要它了。
- 局部变量。

**SqlSessionFactory：**

- 相当于_数据库连接池_。
- SqlSessionFactory一旦被创建就应该在应用的运行期间一直存在，**没有任何理由丢弃它或重新创建另一个实例**。
- 因此SqlSessionFactory的最佳作用域是应用作用域。
- 最简单的就是使用**单例模式**或静态单例模式。

**SqlSession**

- 连接到连接池的一个请求！
- SqlSession的实例不是线程安全的，因此是不能被共享的，所以他的最佳的作用域是请求或方法作用域。
- 用完之后需要赶紧**关闭**，否则资源被占用！_**这个操作是十分重要的。**_

![20220424134447](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424134447.png)
这里面的每一个Mapper，就代表一个具体的业务！

---

​		

# 8、解决属性名和字段名不一致的问题

## 8.1 问题

数据库中的字段

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424134502.png" alt="20220424134502" style="zoom: 50%;" />

新建一个项目，拷贝之前的，测试实体类字段不一致的情况。

```java
public class User {
    private int id;
    private String name;
    private String password;
}
```

测试出现问题：

![20220424134525](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424134525.png)

password字段为null。
**原因是：**

```
select * from mybatis.user where id = #{id};

-- 类型处理器
select id,name,pwd from mybatis.user where id = #{id};
```

​	

## 8.2 解决方法

-  方法一：**起别名**【较为低级，不推荐】 

```xml
<select id="getUserById" resultType="xleixz.pojo.User">
        select id,name,pwd as password
        from mybatis.user
        where id = #{id};
    </select>
```

-  方法二（推荐）：引入resultMap结果集映射【见[resultMap结果集映射](#9、resultMap结果集映射)】 

---

​	

# 9、resultMap结果集映射

> resultMap  结果集映射

```
id  name  pwd
id  name  password
```

结果集映射方法：

```xml
<!--id对应select中的resultMap的名字,type对应映射结果集-->
    <resultMap id="UserMap" type="User">

       <!--column数据中的字段，property实体类中的属性-->
       <!--相同的字段可以不写，只写需要映射的字段即可。
       <result column="id" property="id"/>
       <result column="name" property="name"/>
       -->
        <result column="pwd" property="password"/>
    </resultMap>

<!--使用resultMap类型-->
<select id="getUserById" resultMap="UserMap">
        select *
        from mybatis.user
        where id = #{id};
    </select>
```

- resultMap元素是 MyBatis 中最重要最强大的元素。  
- ResultMap的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。  
- ResultMap最优秀的地方在于，虽然你已经对它相当了解了，但是根本就不需要显示地用到他们。
  - 理解：字段一样的地方可以省略不写，只写需要映射的字段即可。

---

​		

# 10、日志

## 10.1 日志工厂

> Mybaits：[settings（设置）](https://mybatis.org/mybatis-3/zh/configuration.html#settings)

如果一个数据库操作，出现了异常，需要排错。日志就是最好的助手！

以前：sout、debug；

现在：日志工厂。

![20220424134600](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424134600.png)

- SLF4J
- LOG4J【掌握】
- LOG4J2 
- JDK_LOGGING 
- COMMONS_LOGGING 
- STDOUT_LOGGING 【掌握】
- NO_LOGGING  

在Mybatis中具体使用哪一个日志实现，在设置中设定。

核心配置文件顺序：

![20220424134613](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424134613.png)

​	

**STDOUT_LOGGING标准日志输出**
在Mybatis核心配置中，配置日志！

```xml
<settings>
        <!--标准的日志工厂实现-->
        <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

效果：

![20220424134625](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424134625.png)

​	

## 10.2 log4j

> Mybaits官网传送门：[settings（设置）](https://mybatis.org/mybatis-3/zh/configuration.html#settings)

### 10.2.1 什么是Log4j？

> Log4j是Apache的一个开源项目，通过使用Log4j，我们可以控制日志信息输送的目的地是控制台、文件、GUI组件；
> 可以控制每一条日志的输出格式；
> 通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程；
> 通过一个配置文件来灵活地进行配置，不需要修改应用的代码。

​	

### 10.2.2 配置log4j文件

1. 在`pom.xml`中导入log4j的包。

> 传送门：[Maven官网导log4j](https://mvnrepository.com/artifact/log4j/log4j)

```xml
<!-- https://mvnrepository.com/artifact/log4j/log4j -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

2. 添加`log4j.properties`配置文件。

```properties
#将等级为DEBUG的日志信息输出的位置，console为输出到控制台，file为输出到文件,console和file的定义在下面的代码
log4j.rootLogger=DEBUG,console,file

#控制台输出的相关设置
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c]-%m%n

#文件输出的相关设置
log4j.appender.file = org.apache.log4j.RollingFileAppender
log4j.appender.file.File=./log/shun.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n

#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
```

3. 在核心配置文件`Mybatis-config.xml`配置log4j为日志的实现。

```xml
<settings>
    <setting name="logImpl" value="LOG4J"/>
</settings>
```

​	

### 10.2.3 log4j简单测试使用

1. 在要使用Log4j的类中，导入包`import org.apache.log4j.Logger;`，注意：包不能导错，要导Apache的。
1. 日志对象，参数为当前类的class：

```java
static Logger logger = Logger.getLogger(UserMapperTest.class);
```

3. 日志级别。

```java
 @Test
    public void testLog4j(){

        logger.info("info:进入了testLog4j方法");
        logger.debug("debug:进入了testLog4j方法");
        logger.error("error:进入了testLog4j方法");

    }
```

4. 运行后会自动生成log日志文件，相当于在程序中加入`System.out.println(info)`一样，在log日志文件可以清晰的看到日志信息。

![20220424134652](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424134652.png)

​	

### 10.2.4 IDEA无法打开log文件的处理方法

 IDEA有的时候无法打开一些后缀的文件，可以在File-settings-Editor-File Types中添加想要打开的文件类型。 

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424134704.png" alt="20220424134704" style="zoom:50%;" />

​	

再次回到IDEA打开log文件， 此时IDEA会弹出如下提示，也就是安装log文件的插件，点击**Install plugins**安装插件之后，也就可以正常查看文件了。 

![20220424134718](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220424134718.png)

----

​		

# 11、分页

## 11.1 Limit实现分页

> **为什么要分页？**
>
> - 减少数据的处理量

**使用Limit分页：**

```sql
语法 ： select * from user limit startIndex, pageSize;
```

`startIndex`为起始位置，从哪里开始查，`pageSieze`为每页显示多少个。

```sql
select * from user limit 3;
# 相当于select * from user limit 0,3;
```

**使用Mybatis实现分页**，核心为SQL。

1. 接口

```java
List<User> getUserByLimit(Map<String, Integer> map);
```

2. Mapper.xml

```xml
<!--分页实现查询-->
   <select id="getUserByLimit" parameterType="map" resultMap="UserMap">
        select *
        from mybatis.user limit #{startIndex},#{pageSize};
    </select>
```

3. 测试

```java
@Test
    public void getUserByLimit() {

        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        HashMap<String, Integer> map = new HashMap<String, Integer>();

        map.put("startIndex", 0);
        map.put("pageSize", 2);

        List<User> userList =  mapper.getUserByLimit(map);

        for (User user : userList) {
            System.out.println(user);
        }

        sqlSession.close();
    }
```

​	

## 11.2 【不建议使用】RowBounds实现分页

不再使用Sql实现分页

1. 接口

```java
//RowBounds实现分页
    List<User> getUserByRowBounds();
```

2. mapper.xml

```xml
<!--RowBounds实现分页-->
    <select id="getUserByRowBounds" resultMap="UserMap">
        select *
        from mybatis.user;
    </select>
```

3. 测试

```java
@Test
    public void getUserByRowBounds() {

        SqlSession sqlSession = MybatisUtils.getSqlSession();

        //RowBounds实现
        RowBounds rowBounds = new RowBounds(1, 2);


        //通过java代码层面实现分页
        List<User> userList = sqlSession.selectList("xleixz.dao.UserMapper.getUserByRowBounds", null, rowBounds);

        for (User user : userList) {

            System.out.println(user);
        }

        sqlSession.close();
    }
```

​	

## 11.3 分页插件

> 传送门：https://pagehelper.github.io/

![20220423171131](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220423171131.png)

​	

了解即可，当公司的架构师需要使用时，需要知道是什么即可！

---

​		

# 12、使用注解开发

## 12.1 面向接口编程

**面向接口编程含义**

在一个面向对象的系统中，系统的各种功能是由许许多多的不同对象协作完成的。在这种情况下，各个对象内部是如何实现自己的,对系统设计人员来讲就不那么重要了；而各个对象之间的协作关系则成为系统设计的关键。小到不同类之间的通信，大到各模块之间的交互，在系统设计之初都是要着重考虑的，这也是系统设计的主要工作内容。面向接口编程就是指按照这种思想来编程。

**根本原因**：**==解耦==，可拓展，提高复用，分层开发中，上层不用管具体的实现，都遵守共同的标准，使得开发变得容易，规范性好。**

​	

**关于接口的理解**

接口从更深层次的理解，应是定义与实现的分离。

接口的本身反应了系统设计人员对系统的抽象理解。

接口应有两类：

- 第一类是对一个个体的抽象，它可对应一个抽象体；
- 第二类是对一个个体某一方面的抽象，即形成了一个抽象面。

一个个体有可能有多个抽象面。抽象体与抽象面是有区别的。

​	

**三个面向区别**

面向对象是指，我们考虑问题时，以对象为单位，考虑它的属性及方法。

面向过程是指，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现。

接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题。更多的体现就是对系统整体的架构。

​	

## 12.2 使用注解开发

1. 注解在接口上实现

```java
public interface UserMapper {

    @Select("select * from user")
    List<User> getUsers();
}
```

2. 需要在核心配置文件中绑定接口

```xml
<!--绑定接口-->
    <mappers>
        <mapper class="xleixz.dao.UserMapper"/>
    </mappers>
```

3. 测试

```java
  @Test
    public void test(){

       SqlSession sqlSession = MybatisUtils.getSqlSession();

       //底层主要应用反射
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       List<User> users = mapper.getUsers();

       for (User user : users) {
           System.out.println(user);
       }

       sqlSession.close();
   }

```

​	

本质：反射机制实现。

底层：动态代理。

![20220423180646](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220423180646.png)

---

​		

# 13、Mybatis执行流程

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220423182127.jpg" alt="20220423182127" style="zoom: 67%;" />

---

​		

# 14、使用注解实现增删改查

## 14.1 注解实现增删改查

我们可以在工具类创建的时候实现自动提交事务。

==注意：==需要优化工具类` return sqlSessionFactory.openSession(true);`，设置**自动提交事务**。

```java
//工具类
//sqlSessionFactory  工厂模式
public class MybatisUtils {

    private static SqlSessionFactory sqlSessionFactory;
    static {
        try {
            //使用Mybatis第一步，必须要做！！！
            //获取SqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    public static SqlSession getSqlSession() {
        //openSession方法的自动提交设置为true就会自动提交了
        return sqlSessionFactory.openSession(true);
    }
}
```

​	

**编写接口，增加注解**

```java
public interface UserMapper {

    @Select("select * from user")
    List<User> getUsers();

    //方法存在多个参数，所有的参数前面必须加上@Param("")注解
    @Select("select * from user where id = #{id}")
    User getUserById(@Param("id") int id);

    //引用对象不需要写@Param("")
    @Insert("insert into user(id,name,pwd) values(#{id},#{name},#{password})")
    int addUser(User user);

    @Update("update user set name=#{name},pwd=#{password} where id=#{id}")
    int updateUser(User user);

    @Delete("delete from user where id=#{id}")
    int deleteUser(@Param("id") int id);
}
```

​	

**测试类**

```java
public class UserMapperTest {

   //注解实现查询
   @Test
    public void select(){

       SqlSession sqlSession = MybatisUtils.getSqlSession();

       //底层主要应用反射
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);

       User userById =  mapper.getUserById(1);
       System.out.println(userById);

       sqlSession.close();

   }

   //注解实现添加
   @Test
   public void add(){

      SqlSession sqlSession = MybatisUtils.getSqlSession();

      //底层主要应用反射
      UserMapper mapper = sqlSession.getMapper(UserMapper.class);

      mapper.addUser(new User(7,"hello","123456"));


      sqlSession.close();

   }

   //注解实现更新
   @Test
   public void update(){

      SqlSession sqlSession = MybatisUtils.getSqlSession();

      //底层主要应用反射
      UserMapper mapper = sqlSession.getMapper(UserMapper.class);

      mapper.updateUser(new User(6,"two","123456"));

      sqlSession.close();

   }

   //注解实现删除
   @Test
   public void delete(){

      SqlSession sqlSession = MybatisUtils.getSqlSession();

      //底层主要应用反射
      UserMapper mapper = sqlSession.getMapper(UserMapper.class);

      mapper.deleteUser(6);
      mapper.deleteUser(7);
      mapper.deleteUser(8);

      sqlSession.close();
   }
}
```

​	

**==注意：==必须将接口注册到核心配置文件中！**（具体见前面映射器章节）

​	

## 14.2 关于@Param()注解

- 基本类型的参数或者String类型，需要加上。
- 引用类型不需要加。
- 如果只有一个基本类型，可以会略，但是建议加上。
- 在SQL中引用的就是这里的`@Param("")`中设定的属性名。

​		

## 14.3 #{} 和 ${}区别

#{}能够大程度上防止sql注入，用${}无法防止sql注入。

==**能用#{}时尽量用#{}**==。

---

​	

# 15、Lombok的使用

> Lombok是一个可以通过简单的注解形式来帮助我们简化消除一些必须有但显得很臃肿的Java代码的工具，通过使用对应的注解，可以在编译源码的时候生成对应的方法。
>
> 理解：lombok插件用于“偷懒”，相当于Alt＋Insert快捷键，能自动生成无参构造，get、set、toString、hashcode、equals等方法。
>
> 缺点：缺少直观的代码，新手可能看不懂结构。等于看不到直观的get、set方法的代码。插件依赖jar包，没有jar包无法使用。
>
> 官网帮助文档传送门：[下载Lombok](https://projectlombok.org/ "Lombok下载网址")

![20220423202247](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220423202247.png)

​	

1. 在IDEA中安装插件

2. 导入lombok的jar包

> 传送门：https://mvnrepository.com/artifact/org.projectlombok/lombok

```xml
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.22</version>
    <scope>provided</scope>
</dependency>
```

3. 在实体类上加注解即可

```java
@Getter and @Setter
@FieldNameConstants
@ToString
@EqualsAndHashCode
@AllArgsConstructor, @RequiredArgsConstructor and @NoArgsConstructor
@Log, @Log4j, @Log4j2, @Slf4j, @XSlf4j, @CommonsLog, @JBossLog, @Flogger, @CustomLog
@Data
@Builder
@SuperBuilder
@Singular
@Delegate
@Value
@Accessors
@Wither
@With
@SneakyThrows
@val
@var
@UtilityClass
```

​	

举例：

```java
@Data //无参构造，get、set、toString、hashcode、equals
@AllArgsConstructor
@NoArgsConstructor
public class User {

    private int id;
    private String name;
    private String password;
    
}
```

---

​			

# 16、多对一处理

多对一：

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220423213600.png" alt="20220423213600" style="zoom:50%;" />

- 多个学生，对应一个老师；
- 对于学生而言，**关联**：多个学生，关联一个老师【多对一】；
- 对于老师而言，**集合**：一个老师，有很多学生【一对多】。

​		

SQL：

```mysql
CREATE TABLE `teacher` (
`id` INT(10) NOT NULL,
`name` VARCHAR(30) DEFAULT NULL,
PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO teacher(`id`, `name`) VALUES (1, '秦老师');

CREATE TABLE `student` (
`id` INT(10) NOT NULL,
`name` VARCHAR(30) DEFAULT NULL,
`tid` INT(10) DEFAULT NULL,
PRIMARY KEY (`id`),
KEY `fktid` (`tid`),
CONSTRAINT `fktid` FOREIGN KEY (`tid`) REFERENCES `teacher` (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8


INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('1', '小明', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('2', '小红', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('3', '小张', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('4', '小李', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('5', '小王', '1');
```

​		

## 16.1 测试环境搭建

#### 16.1.1 基本环境准备

1. 在pom文件中导入依赖

```xml
<!--导入依赖-->
    <dependencies>
        <!--mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <!--mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <!--junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>
```

​		

2. Mybatis工具类

```java
//工具类
//sqlSessionFactory  工厂模式
public class MybatisUtils {
    private static SqlSessionFactory sqlSessionFactory;
    static {

        try {
            //使用Mybatis第一步，必须要做！！！
            //获取SqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static SqlSession getSqlSession() {
        //openSession方法的自动提交设置为true就会自动提交了
        return sqlSessionFactory.openSession(true);
    }
}

```

​		

3. 核心配置文件Mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!--configuration核心配置文件-->
<configuration>

    <!--引入外部配置文件-->
    <properties resource="db.properties">
    </properties>

    <settings>
        <!--标准的日志工厂实现-->
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

    <environments default="development">
        <environment id="development">
            <!--transactionManager事务管理-->
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <!--防止出现问号-->
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <mapper class="com.xleixz.mapper.TeacherMapper"/>
        <mapper class="com.xleixz.mapper.StudentMapper"/>
    </mappers>

</configuration>
```

​		

4. properties属性文件

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF8
username=root
password=123456
```

​		

#### 16.1.2环境搭建

1. 在pomxml文件中导入Lombok依赖
   - 安装lombok插件
   - 导入jar包

```xml
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.22</version>
    <scope>provided</scope>
</dependency>
```

​		

2. 新建实体类Teacher，Student

```java
@Data
public class Student {

    private int id;
    private String name;

    //学生需要关联一个老师
    private Teacher teacher;
}
```

```java
@Data
public class Teacher {

    private int id;
    private String name;
}
```

​		

3. 建立Mapper接口

```java
public interface StudentMapper {
}
```

```java
public interface TeacherMapper {
    @Select("select * from teacher where id = #{tid}")
    Teacher getTeacher(@Param("tid") int id);
}
```

​		

4. 建立Mapper.xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.xleixz.mapper.StudentMapper">
</mapper>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.xleixz.mapper.TeacherMapper">
</mapper>
```

​		

5. 在核心配置文件中绑定注册Mapper接口或文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!--configuration核心配置文件-->
<configuration>

    <!--引入外部配置文件-->
    <properties resource="db.properties">
    </properties>

    <settings>
        <!--标准的日志工厂实现-->
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

    <environments default="development">
        <environment id="development">
            <!--transactionManager事务管理-->
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <!--防止出现问号-->
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    
    <mappers>
        <mapper class="com.xleixz.mapper.TeacherMapper"/>
        <mapper class="com.xleixz.mapper.StudentMapper"/>
    </mappers>

</configuration>
```

​	

6. 测试

```java
public class MyTest {

    public static void main(String[] args) {

        SqlSession sqlSession = MybatisUtils.getSqlSession();

        TeacherMapper mapper = sqlSession.getMapper(TeacherMapper.class);
        Teacher teacher = mapper.getTeacher(1);

        System.out.println(teacher);

        sqlSession.close();

    }
}
```

​			

#### 16.1.3 目录结构

![20220430220419](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220430220419.png)

​		

## 16.2 按照查询嵌套处理

强化16.1测试环境，查询所有学生对应的老师。

修改StudentMapper.xml文件。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.xleixz.mapper.StudentMapper">

    <!--思路：
    1.查询所有的学生信息
    2.根据查询的信息的tid，寻找对应的老师-->

    <select id="getStudent" resultMap="StudentTeacher">
        select *
        from student;
    </select>
    <resultMap id="StudentTeacher" type="Student">
        <result property="id" column="id"/>
        <result property="name" column="name"/>

        <!--复杂的属性需要单独处理,对象：association,集合：collection-->
        <association property="teacher" column="tid" javaType="teacher" select="getTeacher"/>
    </resultMap>

    <select id="getTeacher" resultType="Teacher">
        select *
        from teacher
        where id = #{id};
    </select>

</mapper>

```

```java
public class MyTest {

    @Test
    public void testStudent(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
        List<Student> studentList = mapper.getStudent();
        for (Student student : studentList) {
            System.out.println(student);
        }
        sqlSession.close();
    }
}
```

​		

![20220430211838](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220430211838.png)

​		

## 16.3 按照结果嵌套处理

==个人推荐使用，因为可以在SQL中调试==

修改StudentMapper.xml文件。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.xleixz.mapper.StudentMapper"> 
<!--按照结果嵌套处理-->
    <select id="getStudent2" resultMap="StudentTeacher2">
        select s.id sid, s.name sname, t.name tname
        from student s,
             teacher t
        where s.tid = t.id;
    </select>
    <resultMap id="StudentTeacher2" type="Student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <association property="teacher" javaType="teacher">
                <result property="name" column="tname"/>
        </association>
    </resultMap>
</mapper>

```

​		

测试类。

```java
public class MyTest {  
@Test
    public void testStudent2(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
        List<Student> studentList = mapper.getStudent2();
        for (Student student : studentList) {
            System.out.println(student);
        }
        sqlSession.close();
    }
}
```

​	

![20220430213520](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220430213520.png)

​	

## 16.4 回顾MySQL多对一查询方式

- 子查询（按照查询嵌套处理）
- 联表查询（按照结果嵌套处理）

---

​			

# 17、一对多处理

比如：一个老师拥有多个学生，对于老师而言就是一对多的关系。一个老师对应多个学生。

1. 仍然是[环境搭建](#16.1 测试环境搭建 "16.1 测试环境搭建")，清除不需要的东西。

实体类Teacher的修改：

```java
@Data
public class Teacher {

    private int id;
    private String name;

    //一个老师拥有多个学生
    private List<Student> students;
}
```

​	

实体类Student的修改：

```java
@Data
@NoArgsConstructor
public class Student {

    private int id;
    private String name;
    private int tid;
}
```

​	

2. 目录结构

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220430230527.png" alt="20220430230527" style="zoom:50%;" />

​	

## 17.1 按照结果嵌套处理

TeacherMapper.xml文件：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.xleixz.mapper.TeacherMapper">

    <!--按结果嵌套处理（联表查询）-->
    <select id="getTeacher" resultMap="TeacherStudent">
        select s.id sid, s.name sname, t.name tname, t.id tid
        from student s,
             teacher t
        where s.tid = t.id
          and t.id = #{tid}
    </select>

    <resultMap id="TeacherStudent" type="Teacher">

        <result property="id" column="tid"/>
        <result property="name" column="tname"/>

        <!--JavaType - 指定属性的类型
        集合中的泛型信息，使用OfType获取-->
        <collection property="students" ofType="Student">
            <result property="id" column="sid"/>
            <result property="name" column="sname"/>
            <result property="tid" column="tid"/>
        </collection>

    </resultMap>
    </mapper>
```

​		

测试类：

```java
 @Test
    public void test1() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        TeacherMapper mapper = sqlSession.getMapper(TeacherMapper.class);
        Teacher teacher = mapper.getTeacher(1);
        System.out.println(teacher);
        sqlSession.close();
    }
```

​		

## 17.2 按照查询嵌套处理

TeacherMapper.xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.xleixz.mapper.TeacherMapper">

    <!--按子查询嵌套处理（子查询）-->
    <select id="getTeacher2" resultMap="TeacherStudent2">
        select *
        from teacher
        where id = #{tid}
    </select>
    <resultMap id="TeacherStudent2" type="Teacher">
        <collection property="students" column="id" javaType="ArrayList" ofType="Student"
                    select="getTeacherByTeacherId"/>
    </resultMap>

    <select id="getTeacherByTeacherId" resultType="Student">
        select *
        from student
        where tid = #{tid}
    </select>


</mapper>
```

​	

测试类：

```java
@Test
    public void test2() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        TeacherMapper mapper = sqlSession.getMapper(TeacherMapper.class);
        Teacher teacher = mapper.getTeacher(1);
        System.out.println(teacher);
        sqlSession.close();
    }
```

![20220430225647](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220430225647.png)

---

​		

# 18、总结多对一和一对多

1. 关联 - association（多对一）

2. 集合 - collection（一对多）
3. JavaType：用来指定实体类中属性我的类型。
4. OfType：用来指定映射到List或集合中的pojo类型，泛型中的约束类型。

​	

注意点：

- 保证SQL的可读性，尽量保证通俗易懂。
- 注意一对多和多对一中，属性名和字段的问题。
- 如果问题不好排查错误，可以使用日志，建议使用Log4j

---

​		

# 19、动态SQL

什么是动态SQL？

> **动态SQL是指根据不同的条件生成不同的SQL语句。**

动态 SQL 是 MyBatis 的强大特性之一。如果你使用过 JDBC 或其它类似的框架，你应该能理解根据不同条件拼接 SQL  语句有多痛苦，例如拼接时要确保不能忘记添加必要的空格，还要注意去掉列表最后一个列名的逗号。利用动态 SQL，可以彻底摆脱这种痛苦。

使用动态 SQL 并非一件易事，但借助可用于任何 SQL 映射语句中的强大的动态 SQL 语言，MyBatis 显著地提升了这一特性的易用性。

> - if
> - choose (when, otherwise)
> - trim (where, set)
> - foreach

​		

## 19.1 环境搭建

**创建一个SQL表，**`字段：id，title，author，create_time，views`。

```mysql
CREATE TABLE `blog` (
`id` varchar(50) NOT NULL COMMENT '博客id',
`title` varchar(100) NOT NULL COMMENT '博客标题',
`author` varchar(30) NOT NULL COMMENT '博客作者',
`create_time` datetime NOT NULL COMMENT '创建时间',
`views` int(30) NOT NULL COMMENT '浏览量'
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

​	

**创建一个基础工程**

目录结构：

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220501220443.png" alt="20220501220443" style="zoom:50%;" />

​			

1. 在`pom.xml`文件导包

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>Mybatis-Study</artifactId>
        <groupId>org.example</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>Mybatis-SQL</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>


    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.22</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>

</project>
```

​		

2. MybatisUtils工具类

```java
//工具类
//sqlSessionFactory  工厂模式

public class MybatisUtils {

    private static SqlSessionFactory sqlSessionFactory;

    static {


        try {
            //使用Mybatis第一步，必须要做！！！
            //获取SqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }


    public static SqlSession getSqlSession() {
        //openSession方法的自动提交设置为true就会自动提交了
        return sqlSessionFactory.openSession(true);
    }
}
```

​		

3. properties属性

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF8
username=root
password=123456
```

​	

4. 核心配置文件`Mybatis-config.xml`

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!--configuration核心配置文件-->
<configuration>

    <!--引入外部配置文件-->
    <properties resource="db.properties">
    </properties>

    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>

        <!--是否开启驼峰命名自动映射，即从经典数据库列名 A_COLUMN 映射到经典 Java 属性名 aColumn。 -->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>

    <!--可以给实体类起别名-->
    <typeAliases>
        <typeAlias type="com.xleixz.pojo.Blog" alias="Blog"/><!--alias起别名-->
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <!--transactionManager事务管理-->
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <!--防止出现问号-->
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <mapper class="com.xleixz.mapper.BlogMapper"/>
    </mappers>

</configuration>

```

​		

5. 编写实体类

```java
@Data
public class Blog {

    private String id;
    private String title;
    private String author;
    private Date createTime;
    private int views;
}
```

​		

6. 实体类对应的`Mapper接口`和`Mapper.xml`文件。

接口：

```java
public interface BlogMapper {

    //插入数据
    //新增一个博客
    int addBlog(Blog blog);
}
```

​	

sql配置文件：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.xleixz.mapper.BlogMapper">

    <insert id="addBlog" parameterType="blog">
        insert into blog(id,title, author, create_time ,views)
        values (#{id},#{title}, #{author}, #{createTime}, #{views});
    </insert>

</mapper>

```

​	

7. 拓展（IDUtils工具类）

```java
//抑制警告，让黄色警告不提示
@SuppressWarnings("all")
public class IDUtils {

    public static String getId() {

        //获取UUID,生成随机数
        return UUID.randomUUID().toString().replaceAll("-", "");
    }
}
```

​	

8. 测试类（初始化数据）

```java
public class MyTest {

    @Test
    public void addBlogTest() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
        Blog blog = new Blog();
        blog.setId(IDUtils.getId());
        blog.setTitle("Mybatis");
        blog.setAuthor("狂神说");
        blog.setCreateTime(new Date());
        blog.setViews(9999);

        mapper.addBlog(blog);

        blog.setId(IDUtils.getId());
        blog.setTitle("Java");
        mapper.addBlog(blog);

        blog.setId(IDUtils.getId());
        blog.setTitle("Spring");
        mapper.addBlog(blog);

        blog.setId(IDUtils.getId());
        blog.setTitle("微服务");
        mapper.addBlog(blog);

        sqlSession.close();
    }
}
```

初始化数据完毕！

​		

## 19.2 IF

> 作用：相当于SQL中的IF语句检索查询
>
> 官网定义的固定格式：`if中的test为固定`
>
> ```xml
> <select id="xxxxxxxx" resultType="Blog">
>         SELECT * FROM BLOG WHERE xxxxx = ‘xxxxxx’
>    	<if test="title != null">
>    		AND title like #{title}
>   	</if>
>    </select>
>   ```

1. 接口

```java
//需求1
//查询博客
    List<Blog> queryBlogIF(Map map);
```

​	

2. Mapper.xml文件编写SQL

```xml
<!--需求1：
根据作者名字和博客名字来查询博客！
如果作者名字为空，那么只根据博客名字查询，反之，则根据作者名来查询
select * from blog where title = #{title} and author = #{author}
-->
<select id="queryBlogIF" parameterType="map" resultType="blog">
        select *
        from blog
        where 1 = 1
        <if test="title != null">
            and title = #{title}
        </if>
        <if test="author != null">
            and author = #{author}
        </if>
    </select>
```

​	

3. 测试类

```java
 @Test
    public void queryBlogIF() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);

        HashMap map = new HashMap();

        map.put("title", "Java");

        List<Blog> blogs = mapper.queryBlogIF(map);
        for (Blog blog : blogs) {

            System.out.println(blog);
        }

        sqlSession.close();
    }
```

根据`map.put`的内容，检索查询。

​	

![20220501223242](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220501223242.png)

​	

## 19.3 choose、when、otherwise

> 有时候，我们不想使用所有的条件，而只是想从多个条件中选择一个使用。针对这种情况，MyBatis 提供了 choose 元素，它有点像 Java 中的 switch 语句。
>
> ```xml
> <select>
> 	select * from 表
>     <where>
>     	<choose>
>             ……
>         </choose>
>         <choose>
>         	and ……
>         </choose>
>         <otherwise>
>         	and ……
>         </otherwise>
>     </where>
> </select>
> ```

只要找到满足的`<when>`，下面的都不会执行，跟Switch中的`case`原理一样。

```xml
<select id="queryBlogchoose" parameterType="map" resultType="blog">
        select * from blog
        <where>
            <choose>
                <when test="title != null">
                    title = #{title}
                </when>
                <when test="author != null">
                    and author = #{author}
                </when>
                <otherwise>
                    and views = #{views}
                </otherwise>
            </choose>
        </where>
    </select>
```

​	

## 19.4 trim(where、set)

**where**

> *where* 元素只会在子元素返回任何内容的情况下才插入 “WHERE” 子句。而且，若子句的开头为 “AND” 或 “OR”，*where* 元素也会将它们`(and)、(or)`去除。
>
> ```xml
> <select>
> 	select * from 表
>     <where>
>         <if title="">
>         	   ……
>         </if>
>     	<if title="">
>             and ……
>         </if>
>     </where>
> </select>
> ```

使用where标签后SQL语句写`select * from 表`即可，无需写where，同时能将if标签中的`and`、`or`去掉，保证字符串正确拼接。相当于`select * from blog where title = #{title}`。

当使用where标签后，<where>里什么都不写，它也会自动取出where。相当于`select * from blog`。

**例如：**

```xml
<select id="queryBlogIF" parameterType="map" resultType="blog">
        select *
        from blog
        <where>
            <if test="title != null">
                title = #{title}
            </if>
            <if test="author != null">
                and author = #{author}
            </if>
        </where>
    </select>
```

​	

**set**

> 用于动态更新语句的类似解决方案叫做 *set*。*set* 元素会动态地在行首插入 SET 关键字，并会**删掉**额外的逗号。
>
> ```xml
> <update>
> 	update 表
>     <set>
>         <if title = "">
>        		 ……,
>         </if>
>         <if title = "">
>         	 ……,
>         </if>
>     </set>
>    		where id = #{id};
> </update>
> ```

**例如：**

```xml
<!--记得加上逗号-->
    <update id="updateBlog" parameterType="map">
        update blog

        <set>
            <if test="title != null">
                title = #{title},
            </if>
            <if test="author != null">
                author = #{author}
            </if>
        </set>
            where id = #{id};
    </update>
```

​	

**trim**

> 与 *set* 元素等价的自定义 *trim* 元素。

```xml
<trim prefix="SET" suffixOverrides=",">
  ...
</trim>
```

​	

**所谓的动态SQL，本质还是SQL语句，只是我们可以在SQL层面，去执行一个逻辑代码。**

​	

## 19.5 foreach

> **不常用**
>
> 动态 SQL 的另一个常见使用场景是对集合进行遍历（尤其是在构建 IN 条件语句的时候）。

准备：将数据库中前三个数据的id修改为1,2,3；

需求：我们需要查询 blog 表中 id 分别为1,2,3的博客信息

1. 接口

```java
List<Blog> queryBlogForeach(Map map);
```

2. 编写SQL语句

```xml
<select id="queryBlogForeach" parameterType="map" resultType="blog">
  select * from blog
   <where>
       <!--
       collection:指定输入对象中的集合属性
       item:每次遍历生成的对象
       open:开始遍历时的拼接字符串
       close:结束时拼接的字符串
       separator:遍历对象之间需要拼接的字符串
       select * from blog where 1=1 and (id=1 or id=2 or id=3)
     -->
       <foreach collection="ids"  item="id" open="and (" close=")" separator="or">
          id=#{id}
       </foreach>
   </where>
</select>
```

3. 测试

```java
@Test
public void testQueryBlogForeach(){
   SqlSession session = MybatisUtils.getSession();
   BlogMapper mapper = session.getMapper(BlogMapper.class);

   HashMap map = new HashMap();
   List<Integer> ids = new ArrayList<Integer>();
   ids.add(1);
   ids.add(2);
   ids.add(3);
   map.put("ids",ids);

   List<Blog> blogs = mapper.queryBlogForeach(map);

   System.out.println(blogs);

   session.close();
}
```

​	

## 19.6 SQL片段

有的时候，我们可能会将一些公共我的部分抽取出来，方便复用。

1. 使用`<SQL>`标签抽取公共的部分。
2. 在需要的地方使用`<include>`标签引用即可。

```xml
 <!--SQL片段-->
    <sql id="if-title-author">
        <if test="title != null">
            title = #{title}
        </if>
        <if test="author != null">
            and author = #{author}
        </if>
    </sql>

<select id="queryBlogIF" parameterType="map" resultType="blog">
        select *
        from blog
        <where>
            <include refid="if-title-author"/>
        </where>
    </select>
```

​	

**注意：**

- 最好基于单表来定义SQL片段！（不要做太复杂的事情）
- 不要存在`<where>`标签。

---

​	

# 20、缓存

## 20.1 简介

> 问题：查询需要连接数据库，耗资源。
>
> 内存：一次查询的结果，给它暂存在一个可以直接取到的地方（内存），再次查询相同数据的时候，直接走缓存，就不用连接数据库了。

1、什么是缓存 [**Cache**]？

- 存在内存中的临时数据。
- 将用户经常查询的数据放在缓存（内存）中，用户去查询数据就不用从磁盘上(关系型数据库数据文件)查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题。

2、为什么使用缓存？

- 减少和数据库的交互次数，减少系统开销，提高系统效率。

3、什么样的数据能使用缓存？

- 经常查询并且不经常改变的数据。【可以使用缓存】
- 不经常查询且经常改变的数据【不可以使用缓存】

​		

## 20.1 Mybatis缓存

Mybatis包含一个非常强大的缓存特性，它可以非常方便地定制和配置缓存。缓存可以极大的提升查询效率。

Mybatis系统中默认定义了两级缓存：**一级缓存**和**二级缓存**

- 默认情况下，只有一级缓存开启。（SqlSession级别的缓存，也称为本地缓存）
- 二级缓存需要手动开启和配置，它是基于namespace级别的缓存。
- 为了提高扩展性，Mybatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存。

​		

## 20.2 一级缓存

一级缓存也叫本地缓存：SqlSession

- 与数据库同一次会话期间查询到的数据会放在本地缓存中。
- 以后如果需要获取相同的数据，直接从缓存中获取，没必要再去查询数据库。

​	

**测试步骤**

1. 开启日志【mybatis-config.xml】！

```xml
  <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
```

2. 编写接口【UserMapper.java】。

```java
//根据id查询用户
User queryUserById(@Param("id") int id);
```

3. 接口对应的Mapper文件【UserMapper.xml】。

```xml
<select id="queryUserById" resultType="user">
  select * from user where id = #{id}
</select>
```

4. 测试在一个Session中查询两次相同的记录【MyTest.java】。

```java
@Test
    public void test1() {

        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        User user = mapper.queryUserById(1);
        System.out.println(user);

        System.out.println("===========");

        User user2 = mapper.queryUserById(1);
        System.out.println(user2);

        System.out.println(user == user2);
        
        sqlSession.close();
    }
```

5. 查看日志输出。

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220505191802.png" alt="20220505191802" style="zoom:50%;" />

​	

**缓存失效的情况：**

1. 查询不同的东西。

2. 增删改操作，可能会改变原来的数据，所以必定会刷新缓存！

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220505211836.png" alt="20220505211836" style="zoom:50%;" />

3. 查询不同的Mapper.xml。

4. 手动清理缓存！

```java
//手动清理缓存
sqlSession.clearCache();
```

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220505212055.png" alt="20220505212055" style="zoom:50%;" />

​	

> 小结：**一级缓存是默认开启的，只在一次SqlSession中有效，也就是拿到连接到关闭连接这个区间段！**
>
> 一级缓存就是一个Map，用的时候取，用完就崩掉不用了。

​	

## 20.3 二级缓存

二级缓存也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存。

基于namespace级别的缓存，一个名称空间，对应一个二级缓存；

工作机制：

- 一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中；
- 如果当前会话关闭了，这个会话对应的一级缓存就没了；但是我们想要的是，会话关闭了，一级缓存中的数据被保存到二级缓存中；
- 新的会话查询信息，就可以从二级缓存中获取内容；
- 不同的mapper查出的数据会放在自己对应的缓存（map）中；

​	

**测试步骤**

1. 开启全局缓存【mybatis-config.xml】。

```xml
<setting name="cacheEnabled" value="true"/>
```

2. 在要使用二级缓存的Mapper中开启【xxxxMapper.xml】（一般`<cache/>`就够了）。

```xml
<!--使用二级缓存-->
<cache/>
```

也可以自定义参数，**官方示例**[查看官方文档](https://mybatis.org/mybatis-3/zh/sqlmap-xml.html#cache)

```xml
<!--使用二级缓存-->
<cache eviction="FIFO" flushInterval="60000" size="512" readOnly="true"/>
<!--这个更高级的配置创建了一个 FIFO 缓存，每隔 60 秒刷新，最多可以存储结果对象或列表的 512 个引用，而且返回的对象被认为是只读的，因此对它们进行修改可能会在不同线程中的调用者产生冲突。-->
```

3. 测试

<font color="red">报错问题：</font>需要将实体类序列化！！！否则就会报错。

<font color="green">解决：</font>在`<cache/>`标签中加入`redonly=true`即可。

```java
@Test
    public void test1() {

        SqlSession sqlSession = MybatisUtils.getSqlSession();
        SqlSession sqlSession2 = MybatisUtils.getSqlSession();

        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        User user = mapper.queryUserById(1);
        System.out.println(user);
        sqlSession.close();


        /*二级缓存*/
        UserMapper mapper2 = sqlSession2.getMapper(UserMapper.class);
        User user2 = mapper2.queryUserById(1);
        System.out.println(user2);

        System.out.println(user == user2);

        sqlSession2.close();
    }
```

当第一次缓存关闭后，会存入二级缓存，无需再查询数据库。

![20220505220933](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220505220933.png)

​		

> 小结：
>
> 只要开启了二级缓存，在同一个Mapper下就有效。
>
> 所有的数据都会先放在一级缓存中；
>
> 只有会话提交或者关闭以后，一级缓存中的数据才会转到二级缓存中。

​	

## 20.4 缓存原理

![20220505221703](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220505221703.png)

​	

## 20.5 自定义缓存EhCache

Ehcache是一种广泛使用的java分布式缓存，用于通用缓存；

现在使用较少，目前已经广泛使用[Redis缓存](https://baike.baidu.com/item/Redis%E7%BC%93%E5%AD%98/18778961#:~:text=Redis%E7%BC%93%E5%AD%98%E6%98%AF,%E7%A7%8D%E8%AF%AD%E8%A8%80%E7%9A%84API%E3%80%82)。

<font color="red">需要：</font>要在应用程序中使用Ehcache，需要引入依赖的jar包【pom.xml】。

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis.caches/mybatis-ehcache -->
<dependency>
   <groupId>org.mybatis.caches</groupId>
   <artifactId>mybatis-ehcache</artifactId>
   <version>1.1.0</version>
</dependency>
```

​	

1. 在Mapper.xml中加入缓存配置。

```xml'
   <cache type = “org.mybatis.caches.ehcache.EhcacheCache” />
```

2. 编写`ehcache.xml`文件，<font color="red">如果在加载时未找到`/ehcache.xml`资源或出现问题</font>，则将使用默认配置。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
        updateCheck="false">
   <!--
      diskStore：为缓存路径，ehcache分为内存和磁盘两级，此属性定义磁盘的缓存位置。参数解释如下：
      user.home – 用户主目录
      user.dir – 用户当前工作目录
      java.io.tmpdir – 默认临时文件路径
    -->
   <diskStore path="./tmpdir/Tmp_EhCache"/>
   
   <defaultCache
           eternal="false"
           maxElementsInMemory="10000"
           overflowToDisk="false"
           diskPersistent="false"
           timeToIdleSeconds="1800"
           timeToLiveSeconds="259200"
           memoryStoreEvictionPolicy="LRU"/>

   <cache
           name="cloud_user"
           eternal="false"
           maxElementsInMemory="5000"
           overflowToDisk="false"
           diskPersistent="false"
           timeToIdleSeconds="1800"
           timeToLiveSeconds="1800"
           memoryStoreEvictionPolicy="LRU"/>
   <!--
      defaultCache：默认缓存策略，当ehcache找不到定义的缓存时，则使用这个缓存策略。只能定义一个。
    -->
   <!--
     name:缓存名称。
     maxElementsInMemory:缓存最大数目
     maxElementsOnDisk：硬盘最大缓存个数。
     eternal:对象是否永久有效，一但设置了，timeout将不起作用。
     overflowToDisk:是否保存到磁盘，当系统当机时
     timeToIdleSeconds:设置对象在失效前的允许闲置时间（单位：秒）。仅当eternal=false对象不是永久有效时使用，可选属性，默认值是0，也就是可闲置时间无穷大。
     timeToLiveSeconds:设置对象在失效前允许存活时间（单位：秒）。最大时间介于创建时间和失效时间之间。仅当eternal=false对象不是永久有效时使用，默认是0.，也就是对象存活时间无穷大。
     diskPersistent：是否缓存虚拟机重启期数据 Whether the disk store persists between restarts of the Virtual Machine. The default value is false.
     diskSpoolBufferSizeMB：这个参数设置DiskStore（磁盘缓存）的缓存区大小。默认是30MB。每个Cache都应该有自己的一个缓冲区。
     diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是120秒。
     memoryStoreEvictionPolicy：当达到maxElementsInMemory限制时，Ehcache将会根据指定的策略去清理内存。默认策略是LRU（最近最少使用）。你可以设置为FIFO（先进先出）或是LFU（较少使用）。
     clearOnFlush：内存数量最大时是否清除。
     memoryStoreEvictionPolicy:可选策略有：LRU（最近最少使用，默认策略）、FIFO（先进先出）、LFU（最少访问次数）。
     FIFO，first in first out，这个是大家最熟的，先进先出。
     LFU， Less Frequently Used，就是上面例子中使用的策略，直白一点就是讲一直以来最少被使用的。如上面所讲，缓存的元素有一个hit属性，hit值最小的将会被清出缓存。
     LRU，Least Recently Used，最近最少使用的，缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存。
  -->

</ehcache>
```

​	

