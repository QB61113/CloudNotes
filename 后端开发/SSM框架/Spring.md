# Spring

---

# 1、Spring简介

**Spring发展**

Spring : 春天 --->给软件行业带来了春天

2002年，Rod Jahnson首次推出了Spring框架雏形interface21框架。

2004年3月24日，Spring框架以interface21框架为基础，经过重新设计，发布了1.0正式版。

很难想象Rod Johnson的学历 , 他是悉尼大学的博士，然而他的专业不是计算机，而是音乐学。

​	

**Spring理念 : 使现有技术更加实用 . 本身就是一个大杂烩 , 整合现有的框架技术。**

Spring官网 : <http://spring.io/>

官方下载地址 : <https://repo.spring.io/libs-release-local/org/springframework/spring/>

GitHub : <https://github.com/spring-projects>

Spring Maven导包：<https://mvnrepository.com/tags/spring?p=2>

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220506182501.png" alt="20220506182501" style="zoom: 50%;" />

​	

在pom.xml文件中导入依赖包。

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.19</version>
</dependency>
```

​	

## 1.1 Spring优点

- Spring是一个开源的免费的框架（容器）！

- Spring是一个轻量级的、非入侵式的框架！（轻量级：本身很小，只需要将包下载到本地即可使用；非入侵式：引入Spring不会改变代码的原来的任何情况，不会对项目产生任何影响。）
- 控制反转（IOC），面向切面编程（AOP）；
- 支持事务的处理，对框架整合的支持。

​	

<font color="green" >**一句话总结：Spring就是一个轻量级的控制反转（IOC）和面向切面编程（AOP）的框架！**</font>

​	

**弊端：发展了太久之后，违背了原来的理念！配置十分繁琐。直到SpringBoot出现才解放！**

​	

## 1.2 Spring组成

Spring 框架是一个分层架构，由 7 个定义良好的模块组成。Spring 模块构建在核心容器之上，核心容器定义了创建、配置和管理 bean 的方式 。

七大模块：

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220507200303.png" alt="image-20220507200300339" style="zoom:50%;" />

​	

## 1.3 拓展SpringBoot和SpringCloud

Spring就是现代化的Java开发！就是基于SPring的开发！

**Spring Boot与Spring Cloud：**

- Spring Boot

  - SpringBoot是一个快速开发的脚手架。

  - 基于SpringBoot可以快速的开发单个微服务。
  - 约定大于配置！

- Spring Cloud
  - Spring Cloud是基于Spring Boot实现的。

​	

目前大部分公司都在使用SpringBoot进行快速开发，学习SpringBoot的前提是需要完全掌握**Spring**及**SpringMVC**！

---

​	

# 2、IOC

## 2.1 IOC理论推导

<font color="green">**分析实现**：</font>先用我们原来的方式写一段实现代码。如下：

**【案例一】**

1. 先写一个UserDao接口

```java
public interface UserDao {
    void getUser();
}
```

2. 再去写UserDaoImpl的实现类

```java
public class UserDaoImpl implements UserDao {
    @Override
    public void getUser() {
        System.out.println("获取用户数据");
    }
}
```

3. 然后去写UserService业务接口

```java
public interface UserService {
    public void getUser();
}
```

4. 最后写UserServiceImpl业务实现类

```java
public class UserServiceImpl implements UserService {
    private UserDao userDao = new UserDaoImpl();

    @Override
    public void getUser() {
        userDao.getUser();
    }
}
```

5. 测试一下

```java
@Test
    public void test(){

        //用户实际调用的是业务层，dao层不需要他们接触！
        UserService service = new UserServiceImpl();
        service.getUser();
    }
```

![20220507213612](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220507213612.png)

​	

<font color="red">**注意：在我们之前的业务中，用户的需求可能会影响我们原来的代码，我们需要根据用户的需求去修改源代码！如果程序代码量十分大，修改代码的成本十分高！**</font>

例如：当Userdao的实现类增加一个：

```java
public class UserDaoMysqlImpl implements UserDao {
    @Override
    public void getUser() {
        System.out.println("MySQL获取用户数据");
    }
}
```

再增一个实现类：

```java
public class UserDaoOracleImpl implements UserDao {

    @Override
    public void getUser() {
        System.out.println("Oracle获取用户数据");
    }
}
```

<font color="#FF0000">不可能每次都去修改【UserServiceImpl】</font>

![20220507214231](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220507214231.png)

​		

<font color="green">**解决：**</font>

1. 修改业务实现类【UserServiceImpl】利用set , 改造代码

```java
//利用set进行动态实现值的注入
public void setUserDao(UserDao userDao) {
    this.userDao = userDao;
}
```

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220507214534.png" alt="20220507214534" style="zoom: 50%;" />

2. 测试类

```java
@Test
    public void test(){

        //用户实际调用的是业务层，dao层不需要他们接触！
        UserService userService = new UserServiceImpl();

        ((UserServiceImpl) userService).setUserDao(new UserDaoImpl());
        userService.getUser();
    }
```

​	

<font color="#3CB371">**总结：**</font>

之前，程序是主动创建对象！控制权在程序员手上！

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220507224019.png" alt="20220507224019" style="zoom: 50%;" />

使用set注入后，程序员不再具有主动性，而是变成了被动的接受对象！

Spring的底层全是set方法机制，如果没有set方法，Spring是跑不起来的。

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220507224038.png" alt="20220507224038" style="zoom:50%;" />

​	

<font color="#3CB371">这种思想，从本质上解决了问题，程序员不用再去管理对象的创建了，系统的耦合性大大降低，可以更加专注在业务实现上。</font>**这是IOC的原型！**

​	

## 2.2 IOC本质

**控制反转IOC（Inversion of Control），是一种设计思想，DI（依赖注入）是实现IOC的一种方式。**

没有IOC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由**程序自己控制**。

控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：**获得依赖对象的方式反转了。**

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220507225349.png" alt="20220507225349" style="zoom:50%;" />

​	

**IOC是Spring框架的核心内容**，使用多种方式完美的实现了IOC，**可以使用XML配置，也可以使用注解，新版本的Spring也可以零配置实现IOC。**

<font color="green">原理过程：</font>**Spring容器在初始化时先读取配置文件，根据配置文件或元数据创建与组织对象存入容器中，程序使用时再从IOC容器中取出需要的对象。**

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220507225708.png" alt="20220507225708" style="zoom:50%;" />

​	

采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

**控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IOC容器，其实现方法是依赖注入（Dependency Injection,DI）。**

---

​	

# 3、Hello Spring【实战基础】

1. 准备一个实体类【Hello】。

   > 这个实体类中必须存在**set**方法，依赖注入就是利用set方法注入的。

```java
public class Hello {
    private String str;

    public String getStr() {
        return str;
    }

    public void setStr(String str) {
        this.str = str;
    }

    @Override
    public String toString() {
        return "Hello{" +
                "str='" + str + '\'' +
                '}';
    }
}
```

2. 配置Spring文件【beans.xml】。

   > 使用标签`<bean>`创建对象；
   >
   > 传统的创建对象：`类型 变量名 = new 类型();` 或者`对象 变量名 = new 对象();`，例如`Hello hello = new Hello();`
   >
   > Spring创建对象：`<bean id="" class="" >`
   >
   > ​											 	`<!--属性-->`
   >
   > ​												` <property name="" value=""/>`
   >
   >  ​									`</bean>`
   >
   > id：变量名（相当于Hello `hello` = new Hello()的hello）
   >
   > class：new 的对象（相当于这个实现类`Hello`）
   >
   > name：set后面的那部分，**首字母小写**（相当于`setStr`中的`str`）
   >
   > property：相当于给对象中的属性设置一个值

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--使用Spring来创建对象，在Spring中这些都被称为Bean

    类型 变量名 = new 类型();
    对象 变量名 = new 对象();
    Hello hello = new Hello();

    id = 变量名
    class = new 的对象;
    property = 相当于给对象中的属性设置一个值

    -->
    <bean id="hello" class="com.xleixz.pojo.Hello">
        <!--属性-->
        <property name="str" value="Hello Spring!"/>
    </bean>

</beans>
```

3. 测试类【MyTest】。

> 获取Spring的上下文对象（固定的），拿到Spring的容器
>
> `ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");`
>
> 需要什么就get什么，我们的对象现在都在Spring中管理了，我们要使用，直接去`context`里面取出来就可以了
>
> `Hello hello = (Hello) context.getBean("hello");`
>
> 如果不想强转可以写成`Hello hello = context.getBean("hello",Hello.class);`

```java
public class MyTest {

    @Test
    public void test1() {
        //获取Spring的上下文对象（固定的），拿到Spring的容器
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");

        //需要什么就get什么
        //我们的对象现在都在Spring中管理了，我们要使用，直接去里面取出来就可以了
        Hello hello = (Hello) context.getBean("hello");/*强转*/
        System.out.println(hello.toString());
    }
}
```

​	

<font color="green">**解析代码：**</font>

- Hello对象是由**Spring**创建的；
- Hello对象的属性是**Spring容器**设置的。

​	

**这个过程就叫控制反转**：

- 控制：谁来控制对象的创建 , 传统应用程序的对象是由程序本身控制创建的 , **而使用Spring后 , 对象是由Spring来创建的**；
- 反转 : 程序本身不创建对象 , 而变成被动的接收对象；
- 依赖注入 : 就是利用**set方法**来进行注入的。

**IOC是一种编程思想，由主动的编程变成被动的接收。**

​	

**用Spring配置修改【案例一】:**

1. 修改【beans.xml】Spring配置。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="mysqlImpl" class="com.xleixz.dao.UserDaoMysqlImpl"/>
    <bean id="oracleImpl" class="com.xleixz.dao.UserDaoOracleImpl"/>
    <bean id="UserServiceImpl" class="com.xleixz.service.UserServiceImpl">
        <!--
		注意: 这里的name并不是属性，而是set方法后面的那部分，首字母小写
        ref：引用Spring容器中创建好的对象
        value：具体的值，基本数据类型！
        -->
        <property name="userDao" ref="mysqlImpl"/>
    </bean>
</beans>
```

2. 【MyTest.java】测试类

```java
@Test
    public void test() {
        //获取ApplicationContext ：拿到Spring的容器
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        //需要什么就get什么
        UserServiceImpl userServiceImpl = (UserServiceImpl) context.getBean("UserServiceImpl");
        userServiceImpl.getUser();
    }
```

​	

从此以后就不需要改动代码了，要实现不同的操作 , 只需要在xml配置文件中进行修改即可。

一句话总结：**对象由Spring 来创建 , 管理 , 装配 ! **	

---

​	

# 4、IOC创建对象的方式

## 4.1 无参构造函数创建对象

> 使用无参构造创建对象，**默认！**

【User.java】

```java
public class User {
    private String name;

    public User() {
        System.out.println("User的无参构造方法");
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void show() {

        System.out.println("name = " + name);

    }
}
```

【beans.xml】

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user" class="com.xleixz.pojo.User">
        <property name="name" value="xleixz"/>
    </bean>
    
</beans>
```

测试类

```java
@Test
public void test(){
   ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
   //在执行getBean的时候,user已经创建好了,通过无参构造
   User user = (User) context.getBean("user");
   //调用对象的方法.
   user.show();
}
```

运行结果

![20220513232359](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220513232359.png)

**结果可以发现，在调用show方法之前，User对象已经通过无参构造初始化了！**

​	

## 4.2 有参构造函数创建对象

为什么要引入有参构造方法来创建？

**Java类中不存在有参构造方法时，默认的是存在无参构造方法，但是一旦写了有参构造方法，不修改beans.xml时会报错，并且程序无法执行。**

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220513233636.png" alt="20220513233636" style="zoom: 67%;" />

![20220513233659](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220513233659.png)

​	

> 通过有参构造方法来创建

方法一：**下标赋值**

```xml
<!--第一种方法：下标赋值-->
    <bean id="user" class="com.xleixz.pojo.User">
        <constructor-arg index="0" value="有参构造，下标赋值"/>
    </bean>
```

​	

方法二：**类型**，【<font color="red">不推荐使用</font>】

```xml
<!--方法二：类型    不建议使用，因为假如出现了两个String类型时，此时会出现错误-->
    <bean id="user" class="com.xleixz.pojo.User">
        <constructor-arg type="java.lang.String" value="有参构造，类型赋值"/>
    </bean>
```

​	

方法三：**直接通过参数名**

```xml
<!--方法三：直接通过参数名-->
    <bean id="user" class="com.xleixz.pojo.User">
        <constructor-arg name="name" value="有参构造，值赋值"/>
    </bean>
```

​	

**总结：在配置文件加载的时候，容器中管理的对象就已经初始化了。**

假如再创建一个实体类【UserT】，使用无参构造，bean对象后，什么都不写，不调用UserT实体类，仍然会被实例化。

1. 实体类【UserT】

```java
public class UserT {

    private String name;


    public UserT() {
        System.out.println("UserT被创建了");
    }


    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void show() {

        System.out.println("name = " + name);

    }

}
```

2. Spring配置文件【beans.xml】

```xml
<bean id="userT" class="com.xleixz.pojo.UserT">

</bean>
```

3. 测试类【MyTest】

```java
public class MyTest {

    public static void main(String[] args) {

        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");

        User user = (User) context.getBean("user");

        user.show();

    }
}
```

![image-20220527233858182](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220527233858182.png)

---

​	

# 5、Spring配置说明

## 5.1 别名

Spring配置文件【beans.xml】

```xml
<!--别名，如果添加了别名，也可以使用别名获取到这个对象-->
<alias name="user" alias="asdfg"/>
```

测试类【MyTest】 

```java
public class MyTest {

    public static void main(String[] args) {

        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");

        User user = (User) context.getBean("asdfg");

        user.show();

    }
}
```

​	

## 5.2 Bean的配置

Spring配置文件【beans.xml】

```xml
 <!--id：bean的唯一标识符，也就是相当于对象名
        class：bean对象所对应的全限定名：包名+类型
        name：也是别名，name可以同时取多个别名(逗号，空格都可以分割)
        -->
    <bean id="userT" class="com.xleixz.pojo.UserT" name="user2,u2">
        <property name="name" value="xleixz"/>

    </bean>
```

测试类【MyTest】

```java
public class MyTest {

    public static void main(String[] args) {

        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");

        UserT user = (UserT) context.getBean("u2");

        user.show();
        
    }
} 
```

​	

## 5.3 import

这个import，一般用于团队开发使用，他可以将多个配置文件，导入合并为一个。

假设项目中有多个人开发，下面这三个人负责不同的类开发，不同的类需要注册在不同的bean中，我们可以利用import将所有人的beans.xml合并为一个总的！

- 张三
- 李四
- 王五
- applicationContext.xml

```xml
<import resource="beans.xml"/>
<import resource="beans2.xml"/>
<import resource="beans3.xml"/>
```

```java
public class MyTest {

    public static void main(String[] args) {

        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

        UserT user = (UserT) context.getBean("u2");

        user.show();

    }
}
```

​	

使用的时候，直接使用总的配置就可以了。

---

​	

# 6、DI依赖注入

## 6.1 构造器注入

[构造器注入方式](#4、IOC创建对象的方式 "点击查看第四章构造器注入方式")

​	

## 6.2 Set方式注入【最重点】

**依赖注入：Set注入！**

- 依赖：bean对象的创建依赖于容器！
- 注入：bean对象中的所有属性，由容器来注入！

​	

**【普通值注入】**

1. 复杂类型【Address.java】

```java
public class Address {

    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}
```

2. 真实测试对象【Student.java】

```java
    private String name;
    private Address address;
    private String [] books;
    private List<String> hobbies;
    private Map<String,String> card;
    private Set<String> games;
    private String wife;
    private Properties info;
```

3. Spring配置文件【beans.xml】

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="student" class="com.xleixz.pojo.Student">
         <!--第一种，普通值注入，value-->
        <property name="name" value="小雷"/>
    </bean>

</beans>
```

4. 测试类【MyTest.java】

```java
public class MyTest {

    public static void main(String[] args) {
        ApplicationContext context  = new ClassPathXmlApplicationContext("beans.xml");
        Student student = (Student)context.getBean("student");

        System.out.println(student.toString());
        System.out.println(student.getName());
        System.out.println(student.getAddress());
    }
}
```

​	

**【Bean注入】**

```xml
<bean id="student" class="com.xleixz.pojo.Student">
        <!--第二种，Bean注入，ref-->
        <property name="address" ref="address"/>
</bean>
```

​	

【**数组注入**】

```xml
<bean id="student" class="com.xleixz.pojo.Student">  
<!--第三种，数组注入，ref-->
        <property name="books">
            <array>
                <value>红楼梦</value>
                <value>西游记</value>
                <value>水浒传</value>
                <value>三国演义</value>
            </array>
        </property>
</bean>
```

​	

【**List注入**】

```xml
<bean id="student" class="com.xleixz.pojo.Student">  
<!--第四种，List注入-->
        <property name="hobbies">
            <list>
                <value>听歌</value>
                <value>看电影</value>
            </list>
        </property>
</bean>
```

​	

【**Map注入**】

```xml
<bean id="student" class="com.xleixz.pojo.Student">  
 <!--第五种，Map注入-->
        <property name="card">
            <map>
                <entry key="身份证" value="123232132323"/>
                <entry key="银行卡" value="79853495495450345"/>
            </map>
        </property>
</bean>
```

​	

【**Set注入**】

```xml
<bean id="student" class="com.xleixz.pojo.Student">  
 <!--第六种，Set注入-->
        <property name="games">
            <set>
                <value>LOL</value>
                <value>GTA5</value>
                <value>PUBG</value>
            </set>
        </property>
</bean>
```

​	

【**null值注入**】

```xml
<bean id="student" class="com.xleixz.pojo.Student">  
 <!--第七种，null值注入-->
        <property name="wife">
            <null/>
        </property>
</bean>
```

​	

【**Properties注入**】

```xml
<bean id="student" class="com.xleixz.pojo.Student">   
<!--第八种，Properties注入-->
        <property name="info">
            <props>
                <prop key="name">小雷</prop>
                <prop key="password">123456</prop>
            </props>
        </property>
</bean>
```

​	

## 6.3 拓展方式注入

我们可以使用`p命名空间`和`c命名空间`进行注入。

【**p命名空间**】

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--P命名空间注入，可以直接注入属性的值：properties-->
    <bean id="user" class="com.xleixz.pojo.User" p:name="小雷test1" p:age="22"/>

</beans>
```

测试：

```java
@Test
    public void test2() {
        ApplicationContext context = new ClassPathXmlApplicationContext("Userbeans.xml");
        User user = context.getBean("user", User.class);
        System.out.println(user);
    }
```

​	

【**c命名空间**】

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--C命名空间注入，可以通过构造器注入：construct-->
    <bean id="user2" class="com.xleixz.pojo.User" c:name="小雷test2" c:age="22"/>

</beans>
```

测试：

```java
  @Test
    public void test3() {
        ApplicationContext context = new ClassPathXmlApplicationContext("Userbeans.xml");
        User user2 = context.getBean("user2", User.class);
        System.out.println(user2);
    }
```

​	

**注意：**p命名空间和c命名空间不能直接使用，需要导入xml约束！！

```xml
xmlns:p="http://www.springframework.org/schema/p"
xmlns:c="http://www.springframework.org/schema/c"
```

​		

## 6.4 Bean的作用域

| Scope                                                        | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [singleton](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes-singleton) | (Default) Scopes a single bean definition to a single object instance for each Spring IoC container. |
| [prototype](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes-prototype) | Scopes a single bean definition to any number of object instances. |
| [request](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes-request) | Scopes a single bean definition to the lifecycle of a single HTTP request. That is, each HTTP request has its own instance of a bean created off the back of a single bean definition. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [session](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes-session) | Scopes a single bean definition to the lifecycle of an HTTP `Session`. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [application](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes-application) | Scopes a single bean definition to the lifecycle of a `ServletContext`. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [websocket](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#websocket-stomp-websocket-scope) | Scopes a single bean definition to the lifecycle of a `WebSocket`. Only valid in the context of a web-aware Spring `ApplicationContext`. |

1. 单例模式（Spring默认机制）

```xml
<bean id="user2" class="com.xleixz.pojo.User" c:age="18" c:name="小雷" scope="singleton"/>
```

2. 原型模式：每次从容器中get的时候，都会产生一个新对象！

```xml
<bean id="accountService" class="com.something.DefaultAccountService" scope="prototype"/>
```

3. 其余的`request`、`session`、`application`这些只能在web开发中使用到！

---

​	

# 7、Bean的自动装配

自动装配是Spring满足bean依赖的一种方式！

Spring会在上下文中自动寻找，并自动给bean装配属性！

​	

在Spring中有三种装配的方式：

1. 在xml中显示配置
2. 在java中显示配置
3. 隐式的自动装配bean【重要】

​		

## 7.1 环境搭建【自动装配准备工作】

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="cat" class="com.xleixz.pojo.Cat"/>
    <bean id="dog" class="com.xleixz.pojo.Dog"/>
    <bean id="people" class="com.xleixz.pojo.People">
          <property name="cat" ref="cat"/>
          <property name="dog" ref="dog"/>
          <property name="name" value="小雷"/>
    </bean>
</beans>
```

```java
public class People {

    private Cat cat;
    private Dog dog;
    private String name;

    public Cat getCat() {
        return cat;
    }

    public void setCat(Cat cat) {
        this.cat = cat;
    }

    public Dog getDog() {
        return dog;
    }

    public void setDog(Dog dog) {
        this.dog = dog;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "People{" +
                "cat=" + cat +
                ", dog=" + dog +
                ", name='" + name + '\'' +
                '}';
    }
}
```

```java
public class Dog {

    public void shout() {
        System.out.println("狗叫");
    }
}
```

```java
public class Cat {
    public void shout(){
        System.out.println("猫叫");
    }
}
```

```java
public class MyTest {

    @Test
    public void test1() {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        People people = context.getBean("people", People.class);

        people.getCat().shout();
        people.getDog().shout();
        System.out.println(people.getName());
    }
}
```

​	 

## 7.2 byName自动装配

> byName：会自动在容器上下文查找，找set方法后对应的的值相对应的bean id。
>
> 优点：不会因为类型冲突而无法使用，一个set方法后的值对应一个id。
>
> 缺点：bean id的名字必须和set方法后的值一样，否则会找不到该对象。

```xml
<bean id="cat" class="com.xleixz.pojo.Cat"/>
<bean id="dog" class="com.xleixz.pojo.Dog"/>

<!--byName：会自动在容器上下文查找，和自己对象set方法后面的值对应的bean id-->
<bean id="people" class="com.xleixz.pojo.People" autowire="byName">
		<property name="name" value="小雷"/>
</bean>
```

​	

## 7.3 byType自动装配

> byType：会自动在容器上下文查找，找和自己对象属性类型相同的bean。
>
> 优点：可以省略bean中的id不写，当类型不冲突时，对id的命名没有要求。
>
> 缺点：当类型冲突时，假如有两个dog，就会报错不能使用。

```xml
<bean id="cat" class="com.xleixz.pojo.Cat"/>
<bean id="dog232323" class="com.xleixz.pojo.Dog"/>

<!--byType：会自动在容器上下文查找，找和自己对象属性类型相同的bean。-->
<bean id="people" class="com.xleixz.pojo.People" autowire="byType">
    <property name="name" value="小雷"/>
</bean>
```

​	

## 7.4 使用注解进行自动装配

jdk1.5支持注解，Spring2.5支持注解。

使用注解注意条件：

1. 导入约束（`bean标签中的网址`）；

2. 配置注解支持。（`<context:annotation-config/>`）【**重要！！不能忘记**】

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">
	<!--开启注解的支持-->
    <context:annotation-config/>

</beans>
```

​	

**@AutoWired注解**

> @Autowired是按类型自动转配的，不支持id匹配。
>
> 直接在属性使用，也可以在set方法上使用；
>
> 可以省略set方法不写，但是不能省略get方法！

【实体类】

```java
public class People {
    @Autowired
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;

    public Cat getCat() {
        return cat;
    }
    @Autowired
    public void setCat(Cat cat) {
        this.cat = cat;
    }
}
```

【配置文件】

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <!--开启注解的支持-->
    <context:annotation-config/>

    <bean id="cat" class="com.xleixz.pojo.Cat"/>
    <bean id="dog232323" class="com.xleixz.pojo.Dog"/>
    <bean id="people" class="com.xleixz.pojo.People"/>
</beans>
```

​	

【拓展，了解】

> 即使值为null，也不会报错。

**@Autowired(required=false)**  说明：false，对象可以为null；true，对象必须存对象，不能为null。

```java
//如果允许对象为null，设置required = false,默认为true
@Autowired(required = false)
private Cat cat;
```

​	

**@Qualifier注解**

> @Autowired是根据类型自动装配的，加上@Qualifier则可以根据byName的方式自动装配；
>
> @Qualifier不能单独使用。

测试实验步骤：

1、配置文件中bean的id中的相同类型较多时。

```xml
<bean id="dog1" class="com.kuang.pojo.Dog"/>
<bean id="dog2" class="com.kuang.pojo.Dog"/>
<bean id="cat1" class="com.kuang.pojo.Cat"/>
<bean id="cat2" class="com.kuang.pojo.Cat"/>
```

2、没有加Qualifier测试，直接报错。

3、在属性上添加`@Qualifier`注解。

```java
@Autowired
@Qualifier(value = "cat2")
private Cat cat;
@Autowired
@Qualifier(value = "dog2")
private Dog dog;
```

测试，成功输出！

​	

 **@Resource注解**

> @Resource如有指定的name属性，先按该属性进行byName方式查找装配；
>
> 其次再进行默认的byName方式进行装配；
>
> 如果以上都不成功，则按byType的方式自动装配。
>
> 都不成功，则报异常。

【实体类】

```java
public class User {
   //如果允许对象为null，设置required = false,默认为true
   @Resource(name = "cat2")
   private Cat cat;
   @Resource
   private Dog dog;
   private String str;
}
```

【beans.xml】

```xml
<bean id="dog" class="com.kuang.pojo.Dog"/>
<bean id="cat1" class="com.kuang.pojo.Cat"/>
<bean id="cat2" class="com.kuang.pojo.Cat"/>

<bean id="user" class="com.kuang.pojo.User"/>
```

测试：结果OK

配置文件2：beans.xml ， 删掉cat2

```xml
<bean id="dog" class="com.kuang.pojo.Dog"/>
<bean id="cat1" class="com.kuang.pojo.Cat"/>
```

实体类上只保留注解

```java
@Resource
private Cat cat;
@Resource
private Dog dog;
```

结果：OK

结论：先进行byName查找，失败；再进行byType查找，成功。

​	

**小结**：@Autowired与@Resource异同：

> - @Autowired与@Resource都可以用来装配bean。都可以写在字段上，或写在setter方法上。
>
> - @Autowired通过**byType**的方式实现；【常用】
> - @Resource默认通过**byName**的方式实现，如果名字找不到，则通过byType实现，如果两个都找不到，会报错。【常用】

---

​	

# 8、使用注解开发

在Spring4之后，要使用注解开发，必须要保证AOP包导入了。

![image-20220601213317282](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220601213317282.png)

使用注解需要导入context约束，增加注解的支持。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">
    <!--开启注解的支持-->
    <context:annotation-config/>

</beans>
```

​	

> bean的实现

1. 扫描指定包：【applicationContext.xml】配置文件

```xml
<!--指定要扫描的包，这个包下的注解就会生效-->
<context:component-scan base-package="com.xleixz"/>
```

2. 在指定包下添加注解@Component 【User.java】

```java
//等价于 <bean id="user" class="com.xleixz.pojo.User"/>
@Component
public class User {
    public String name = "小雷";
}
```

3. 测试

```java
@Test
    public void test1() {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        User user = context.getBean("user", User.class);
        System.out.println(user.name);
    }
```

​	

> 属性注入

1. 可以不用提供set方法，直接在直接名上添加@value("值")。

```java
//等价于 <bean id="user" class="com.xleixz.pojo.User"/>
@Component
public class User {

    //等价于 <property name="name" value="xleixz"/>
    @Value("小雷")
    public String name;
}
```

2. 如果提供了set方法，在set方法上添加@value("值");

```java
//等价于 <bean id="user" class="com.xleixz.pojo.User"/>
@Component
public class User {
    
    public String name;
    
    //等价于 <property name="name" value="xleixz"/>
    @Value("小雷")
    public void setName(String name) {
       this.name = name;
	}
}
```

​	

> 衍生注解

@Component有几个衍生注解，在web开发中，会按照MVC三层架构分层！

- dao【@Repository】
- service【@Service】
- controller【@Controller】

这四个注解功能都是一样的，都是代表将某个类注册到Spring中，装配Bean

​	

> 自动装配

- @AutoWired：自动装配通过类型，名字；
- @Nullable：字段标记了这个注解，说明这个字段可以为null；
- @Resource：自动装配通过名字，类型。

详情见[自动装配](# 7、Bean的自动装配 "点击查看自动装配")

​	

> 作用域

- singleton：默认的，Spring会采用单例模式创建这个对象。关闭工厂 ，所有的对象都会销毁。
- prototype：多例模式。关闭工厂 ，所有的对象不会销毁。内部的垃圾回收机制会回收。

```java
//作用域
@Scope("singleton")
public class User {
}
```

​		

> 小结

**xml 与 注解**

- xml  ：更加万能，适用于任何场合！维护简单方便；
- 注解：不是自己类实现不了，维护相对复杂，开发简单方便。

**xml与注解整合开发** 

- xml管理bean；
- 注解只负责完成属性注入；
- 开发过程中，只需要注意一个问题：必须让注解生效，就需要开启注解的支持。

```xml
<!--开启注解的支持-->
<context:annotation-config/>

<!--指定要扫描的包，这个包下的注解就会生效-->
<context:component-scan base-package="com.xleixz"/>
```

---

​	

# 9、使用Java的方式配置Spring

这章节完全不使用Spring的xml配置，全权交给Java来做！

JavaConfig是Spring的一个子项目，在Spring4之后它成为了一个全新的功能。

​	

【实体类】

```java
//这个注解的意思就是说明这个类被Spring接管了，注册到了容器中
@Component
public class User {
    private String name;

    public String getName() {
        return name;
    }
    //属性注入值
    @Value("小雷")
    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

【配置文件】

```java
//这个也会被Spring容器托管，注册到容器中，因为它本来就是一个Component
//@Configuration 就是一个配置类，等价于beans.xml或者applicationContext.xml
@Configuration
@ComponentScan("com.xleixz")

//导入其他配置类
@Import(MyConfig2.class)
public class MyConfig {

    //注册一个bean，等价于<bean id="user" class="com.xleixz.pojo.User"/>
    //这个方法的名字就相当于这个bean的id
    //这个方法的返回值就相当于这个bean的class
    @Bean
    public User getUser() {
        //return就是返回要注入到bean中的对象
        return new User();
    }
}
```

```java
@Configuration
public class MyConfig2 {
}
```

【测试类】

```java
public class MyTest {

    @Test
    public void test1() {
        //如果完全使用了配置方式去做，只能通过AnnotationConfigApplicationContext来获取容器
        //通过配置类的class被加载到容器中
        ApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);
        User getUser = context.getBean("getUser", User.class);
        System.out.println(getUser.getName());
    }
}
```

---

​		

# 10、AOP



