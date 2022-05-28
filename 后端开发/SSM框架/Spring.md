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

# 3、Hello Spring

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

方法二：**类型**，【不推荐使用】

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

​	

## 6.3 拓展方式注入

