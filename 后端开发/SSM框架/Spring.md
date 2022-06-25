# Spring

![Spring02](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/Spring02.png)

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

<font color="red">**注意：在之前的业务中，用户的需求可能会影响原来的代码，需要根据用户的需求去修改源代码！如果程序代码量十分大，修改代码的成本十分高！**</font>

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
   > ​									`</bean>`
   >
   > id：变量名（相当于Hello `hello` = new Hello()的hello）
   >
   > class：new 的对象（相当于这个实现类`Hello`）
   >
   > name：set后面的那部分，**首字母小写**（相当于`setStr`中的`str`）
   >
   > property：相当于给对象中的属性设置一个值
   >
   > ref：引用Spring容器中创建好的对象
   >
   > value：具体的值，基本数据类型！

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

    @Override
    public String toString() {
        return "Address{" +
                "address='" + address + '\'' +
                '}';
    }
}
```

2. 真实测试对象【Student.java】

```java
public class Student {

    private String name;
    private Address address;
    private String [] books;
    private List<String> hobbies;
    private Map<String,String> card;
    private Set<String> games;
    private String wife;
    private Properties info;


    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public String[] getBooks() {
        return books;
    }

    public void setBooks(String[] books) {
        this.books = books;
    }

    public List<String> getHobbies() {
        return hobbies;
    }

    public void setHobbies(List<String> hobbies) {
        this.hobbies = hobbies;
    }

    public Map<String, String> getCard() {
        return card;
    }

    public void setCard(Map<String, String> card) {
        this.card = card;
    }

    public Set<String> getGames() {
        return games;
    }

    public void setGames(Set<String> games) {
        this.games = games;
    }

    public String getWife() {
        return wife;
    }

    public void setWife(String wife) {
        this.wife = wife;
    }

    public Properties getInfo() {
        return info;
    }

    public void setInfo(Properties info) {
        this.info = info;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", address=" + address.toString() +
                ", books=" + Arrays.toString(books) +
                ", hobbies=" + hobbies +
                ", card=" + card +
                ", games=" + games +
                ", wife='" + wife + '\'' +
                ", info=" + info +
                '}';
    }
}
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
<!--第三种，数组注入，array-->
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

​		

## 7.2 byName自动装配

> byName：会自动在容器上下文查找，**找set方法后对应的的值相对应的bean id**。
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

> byType：会自动在容器上下文查找，**找和自己对象属性类型相同的bean**。
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

> @Autowired是**按类型自动装配的，不支持id匹配**。
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

> @Autowired是根据类型自动装配的，**加上@Qualifier则可以根据byName的方式自动装配；**
>
> **@Qualifier不能单独使用。**

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

> @Resource如有指定的name属性，**默认通过byName的方式实现，如果名字找不到，则通过byType实现**，如果
>
> 两个都找不到，会报错。【常用】

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

​	

使用注解需要导入`context约束`，`增加注解的支持`。

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

​	

## 【特别注意】context:annotation-config和 context:component-scan的作用与区别

- `context:annotation-config`是用于激活那些已经在spring容器里注册过的bean（无论是通过xml的方式还

  是通过packagesanning的方式）上面的注解。(激活@Resource和@Autowired注解)

- `context:component-scan`除了具有context:annotation-config的功能之外，`context:component-scan`还

  可以在指定的package下扫描以及注册javabean 。(激活@Resource和@Autowired注解，同时可以配置扫描的包

  以激活@Service、@Controller等注解)

- **开发中使用`context:component-scan`足以**

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

# 10、代理模式

为什么要学习代理模式？因为这就是SpringAOP的底层！【面试：SpringAOP 和 SpringMVC 】

代理模式的分类：

- 静态代理
- 动态代理

 	

理解：

![image-20220605091252574](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220605091252574.png)

​	

## 10.1 静态代理

角色分析：

- 抽象角色：一般会使用接口或抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真实角色，代理真实角色后，一般会做一些附属操作
- 客户：访问代理对象的人

​		

1. 接口

```java
//房屋接口
public interface Rent {

    //要出租的房屋
    public void rent();

}
```

2. 真实角色

```java
//房东
public class Host implements Rent {

    @Override
    public void rent() {
        System.out.println("房东出租房子");
    }

}
```

3. 代理角色

```java
//代理角色
public class Proxy implements Rent {

    private Host host;

    public Proxy(Host host) {
        this.host = host;
    }

    public Proxy() {
    }

    @Override
    public void rent() {
        host.rent();
        seeHouse();
        free();
        hetong();
    }

    //看房
    public void seeHouse() {
        System.out.println("中介带你看房");
    }

    //收中介费
    public void free() {
        System.out.println("收中介费");
    }

    //签租赁合同
    public void hetong() {
        System.out.println("签租赁合同");
    }

}
```

4. 客户端访问这个代理角色

```java
public class Client {

    public static void main(String[] args) {

        //房东租房子
        Host host = new Host();

        //代理，中介帮房东租房子，但是代理角色一般会有一些附属操作
        Proxy proxy = new Proxy(host);

        //租房的人无需面对房东，直接找代理（中介）租房即可
        proxy.rent();
    }
}
```

​	

**代理模式的好处：**

- 可以使真实角色的操作更加纯粹！不用去关注一些公共的业务；
- 公共业务就交给了代理角色，实现了业务的分工；
- 公共业务拓展时，方便集中管理。

**代理模式的缺点：**

- 一个真实角色就会产生一个代理角色；代码量翻倍，开发效率会变低。

​	

## 10.2 静态代理加深理解

1. 接口

```java
public interface UserService {
    public void add();

    public void delete();

    public void update();

    public void query();
}
```

2. 真实角色

```java
//真实对象
public class UserServiceImpl implements UserService {

    @Override
    public void add() {
        System.out.println("增加一个用户");
    }

    @Override
    public void delete() {
        System.out.println("删除一个用户");
    }

    @Override
    public void query() {
        System.out.println("查询一个用户");
    }

    @Override
    public void update() {
        System.out.println("修改一个用户");
    }
}
```

3. 代理角色

```java
public class UserServiceProxy implements UserService {

    private UserServiceImpl userService;

    public void setUserService(UserServiceImpl userService) {
        this.userService = userService;
    }

    @Override
    public void add() {
        log("add");
        userService.add();
    }

    @Override
    public void delete() {
        log("delete");
        userService.delete();
    }

    @Override
    public void update() {
        log("update");

        userService.update();
    }

    @Override
    public void query() {
        log("query");

        userService.query();
    }

    //日志方法
    public void log(String msg) {
        System.out.println("[Debug] 使用了" + msg + "方法");
    }
}
```

4. 客户端访问

```java
public class Client {
    public static void main(String[] args) {

        UserServiceImpl userService = new UserServiceImpl();

        UserServiceProxy userServiceProxy = new UserServiceProxy();
        userServiceProxy.setUserService(userService);

        userServiceProxy.add();
    }
}
```

​	

**所谓的代理模式就是一个业务程序从dao层，开发到Service层，到Controller层，到前端开发。如果需要拓展应用，修改底层代码的风险是巨大的，这时候就可以通过代理模式横切编程。这也是AOP的实现机制！**

​	

## 10.3 动态代理

- 动态代理和静态代理角色一样；
- 动态代理的代理类是动态生成的，不是直接写好的。
- 动态代理分为两大类：
  - 基于接口的动态代理：JDK动态代理【使用】
  - 基于类的动态代理：cglib
  - java字节码实现：Javassist

​	

需要了解两个类：Proxy：代理，InvocationHandler：调用处理程序

​	

编写一个通用的动态代理实现的类！所有的代理对象设置为Object即可！

```java
//自动生成代理类
public class ProxyInvocationHandler implements InvocationHandler {


    private Object target;

    public void setTarget(Object target) {
        this.target = target;
    }

    //生成代理类
    public Object getProxy() {
        return Proxy.newProxyInstance(this.getClass().getClassLoader(),
                target.getClass().getInterfaces(), this);
    }

    // proxy : 代理类
    // method : 代理类的调用处理程序的方法对象.
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        log(method.getName());
        Object result = method.invoke(target, args);
        return result;
    }

    public void log(String methodName) {
        System.out.println("执行了" + methodName + "方法");
    }

}
```

测试

```java
public class Test {
   public static void main(String[] args) {
       //真实对象
       UserServiceImpl userService = new UserServiceImpl();
       //代理对象的调用处理程序
       ProxyInvocationHandler pih = new ProxyInvocationHandler();
       pih.setTarget(userService); //设置要代理的对象
       UserService proxy = (UserService)pih.getProxy(); //动态生成代理类！
       proxy.delete();
  }
}
```

​	

**动态代理的优点**

静态代理有的它都有，静态代理没有的，它也有！

- 可以使得真实角色更加纯粹 . 不再去关注一些公共的事情 .
- 公共的业务由代理来完成 . 实现了业务的分工 ,
- 公共业务发生扩展时变得更加集中和方便 .
- 一个动态代理 , 一般代理某一类业务
- 一个动态代理可以代理多个类，代理的是接口！

---

​	

# 11、AOP

## 11.1 什么是AOP

AOP（Aspect Oriented Programming）意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

​	

## 11.2 AOP在Spring中的作用

**提供声明式事务；允许用户自定义切面。**

以下名词需要了解下：

- 横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志 , 安全 , 缓存 , 事务等等 ....
- 切面（ASPECT）：横切关注点 被模块化 的特殊对象。即，它是一个类。
- 通知（Advice）：切面必须要完成的工作。即，它是类中的一个方法。
- 目标（Target）：被通知对象。
- 代理（Proxy）：向目标对象应用通知之后创建的对象。
- 切入点（PointCut）：切面通知 执行的 “地点”的定义。
- 连接点（JointPoint）：与切入点匹配的执行点。

​	

## 11.3 使用Spring实现AOP

【重点】导入依赖包：[https://mvnrepository.com/artifact/org.aspectj/aspectjweaver](https://mvnrepository.com/artifact/org.aspectj/aspectjweaver "点击到官网查看")

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
   <groupId>org.aspectj</groupId>
   <artifactId>aspectjweaver</artifactId>
   <version>1.9.4</version>
</dependency>
```

​	

**方式一：使用Spring的API接口**

首先编写我们的业务接口和实现类

```java
public interface UserService {
    public void add();

    public void delete();

    public void update();

    public void select();
}
```

```java
public class UserServiceImpl implements UserService {
    @Override
    public void add() {
        System.out.println("增加了一个用户");
    }

    @Override
    public void delete() {
        System.out.println("删除了一个用户");
    }

    @Override
    public void update() {
        System.out.println("更新了一个用户");
    }

    @Override
    public void select() {
        System.out.println("查询了一个用户");
    }
}
```

然后写增强类 , 编写两个 , 一个前置增强 一个后置增强

```java
public class Log implements MethodBeforeAdvice {

    //method:目标对象的方法
    //objects:目标对象的参数
    //target:目标对象
    @Override
    public void before(Method method, Object[] args, Object target) throws Throwable {

        System.out.println(target.getClass().getName() + "的" + method.getName() + "()" + "方法开始执行");
    }
}
```

```java
public class AfterLog implements AfterReturningAdvice {

    @Override
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName() + "的" + method.getName() + "()" + "方法执行完毕，返回值为：" + returnValue);
    }
}
```

在Spring中注册并实现aop切入实现 , 注意导入约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--注册bean-->
    <bean id="userService" class="com.xleixz.service.UserServiceImpl"/>
    <bean id="log" class="com.xleixz.log.Log"/>
    <bean id="afterLog" class="com.xleixz.log.AfterLog"/>

    <!--方式一：使用原生Spring API接口-->
    <!--aop的配置-->
    <aop:config>
        <!--切入点 expression:表达式匹配要执行的方法, execution:要执行的位置-->
        <aop:pointcut id="pointcut" expression="execution(* com.xleixz.service.UserServiceImpl.*(..))"/>

        <!--执行环绕; advice-ref执行方法 . pointcut-ref切入点-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
    </aop:config>

</beans>
```

测试类

```java
public class MyTest {
    @Test
    public void test() {
        ApplicationContext context = new ClassPathXmlApplicationContext("application.xml");
        //动态代理，代理的是接口
        UserService userService = context.getBean("userService", UserService.class);
        userService.add();

    }
}
```

​	

Spring的Aop就是将公共的业务 (日志 , 安全等) 和领域业务结合起来 , 当执行领域业务时 , 将会把公共业务加进来，实现公共业务的重复利用 . 领域业务更纯粹 , 程序猿专注领域业务 , 其本质还是动态代理。

​	

【建议使用】**方式二：自定义类来实现AOP**

编写一个切入类

```java
public class DIYPointCut {

    public void before() {
        System.out.println("===========方法执行前===========");
    }

    public void after() {
        System.out.println("===========方法执行后===========");
    }
}
```

Spring配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--注册bean-->
    <bean id="userService" class="com.xleixz.service.UserServiceImpl"/>
    <bean id="log" class="com.xleixz.log.Log"/>
    <bean id="afterLog" class="com.xleixz.log.AfterLog"/>

    <!--方式二：自定义类-->
    <bean id="diy" class="com.xleixz.DIY.DIYPointCut"/>

    <aop:config>
        <!--自定义切面，ref要引用类-->
        <aop:aspect ref="diy">
            <!--切入点-->
            <aop:pointcut id="pointcut" expression="execution(* com.xleixz.service.UserServiceImpl.*(..))"/>

            <!--前置通知-->
            <aop:before pointcut-ref="pointcut" method="before"/>

            <!--后置通知-->
            <aop:after pointcut-ref="pointcut" method="after"/>

        </aop:aspect>
    </aop:config>

</beans>
```

测试类

```java
public class MyTest {
    @Test
    public void test() {
        ApplicationContext context = new ClassPathXmlApplicationContext("application.xml");
        //动态代理，代理的是接口
        UserService userService = context.getBean("userService", UserService.class);
        userService.add();

    }
}
```

​	

**方式三：使用注解实现**

编写一个增强类

```java
//标注这个类是一个切面类
@Aspect
public class AnnotationPointcut {

    @Before("execution(* com.xleixz.service.UserServiceImpl.*(..))")
    public void before() {
        System.out.println("===========方法执行前===========");
    }

    @After("execution(* com.xleixz.service.UserServiceImpl.*(..))")
    public void after() {
        System.out.println("===========方法执行后===========");
    }

    //在环绕增强中，可以给定一个参数，代表要获取处理切入的点
    @Around("execution(* com.xleixz.service.UserServiceImpl.*(..))")
    public void around(ProceedingJoinPoint jp) throws Throwable {

        System.out.println("环绕前");
        System.out.println("签名:" + jp.getSignature());

        //执行目标方法proceed
        Object proceed = jp.proceed();

        System.out.println("环绕后");
        System.out.println(proceed);
    }
}
```

在Spring配置文件中，注册bean并增加支持注解的配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--注册bean-->
    <bean id="userService" class="com.xleixz.service.UserServiceImpl"/>
    <bean id="log" class="com.xleixz.log.Log"/>
    <bean id="afterLog" class="com.xleixz.log.AfterLog"/>

    <!--方式三：使用注解实现-->
    <bean id="annotationPointcut" class="com.xleixz.DIY.AnnotationPointcut"/>

    <!--开启注解支持-->
    <aop:aspectj-autoproxy/>

</beans>
```

测试

```java
public class MyTest {
    @Test
    public void test() {
        ApplicationContext context = new ClassPathXmlApplicationContext("application.xml");
        //动态代理，代理的是接口
        UserService userService = context.getBean("userService", UserService.class);
        userService.add();

    }
}
```

---

​	

# 12、Mybatis-Spring（整合Mybatis）

## 12.1 整合前环境搭建

1. 导入相关jar包和配置Maven静态资源过滤

   junit

   ```xml
   <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
   </dependency>
   ```

   mybatis

   ```xml
   <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.2</version>
   </dependency>
   ```

   mysql-connector-java

   ```xml
   <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.47</version>
   </dependency>
   ```

   spring相关

   ```xml
   <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.1.10.RELEASE</version>
   </dependency>
   <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.1.10.RELEASE</version>
   </dependency>
   ```

   aspectJ AOP 织入器

   ```xml
   <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
   <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.9.4</version>
   </dependency>
   ```

   mybatis-spring整合包 【重点】

   ```xml
   <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>2.0.2</version>
   </dependency>
   ```

   配置Maven静态资源过滤问题！

   ```xml
   <build>
      <resources>
          <resource>
              <directory>src/main/java</directory>
              <includes>
                  <include>**/*.properties</include>
                  <include>**/*.xml</include>
              </includes>
              <filtering>true</filtering>
          </resource>
      </resources>
   </build>
   ```

   ​	

   **整合**【pom.xml】

   ```xml
   <dependencies>
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
               <scope>test</scope>
           </dependency>
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>5.1.46</version>
           </dependency>
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis</artifactId>
               <version>3.5.2</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-beans</artifactId>
               <version>5.3.19</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>5.1.10.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-jdbc</artifactId>
               <version>5.1.10.RELEASE</version>
           </dependency>
   
           <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
           <dependency>
               <groupId>org.aspectj</groupId>
               <artifactId>aspectjweaver</artifactId>
               <version>1.9.4</version>
           </dependency>
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis-spring</artifactId>
               <version>2.0.2</version>
           </dependency>
           <dependency>
               <groupId>log4j</groupId>
               <artifactId>log4j</artifactId>
               <version>1.2.17</version>
           </dependency>
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <version>1.18.22</version>
           </dependency>
       </dependencies>
   
       <build>
           <resources>
               <resource>
                   <directory>src/main/java</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                   </includes>
                   <filtering>true</filtering>
               </resource>
           </resources>
       </build>
   ```

2. 准备SQL工具类和属性文件

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

   ```properties
   driver=com.mysql.jdbc.Driver
   url=jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF8
   username=root
   password=123456
   ```

3. 准备【mybatis-config.xml】核心配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
   
       <properties resource="db.properties">
       </properties>
   
       <typeAliases>
           <typeAlias type="com.xleixz.pojo.User" alias="User"/><!--alias起别名-->
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
           <mapper class="com.xleixz.mapper.UserMapper"/>
       </mappers>
   </configuration>
   ```

4. 实体类

   ```java
   @Data
   public class User {
       private int id;
       private String name;
       private String pwd;
   }
   ```

5. Mapper接口和接口对应的Mapper映射文件【每一个Mapper.xml都需要在Mybatis核心配置文件中注册】

   ```java
   public interface UserMapper {
   
       public List<User> selectUser();
   }
   ```

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   
   <mapper namespace="com.xleixz.mapper.UserMapper">
   
       <select id="selectUser" resultType="User">
           select * from user
       </select>
   </mapper>
   ```

6. 测试类

   ```java
   public class MyTest {
   
       @Test
       public void select() {
   
           SqlSession sqlSession = MybatisUtils.getSqlSession();
   
           //底层主要应用反射
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
           List<User> userList = mapper.selectUser();
           for (User user : userList) {
               System.out.println(user);
           }
   
           sqlSession.close();
       }
   }
   ```

​	目录结构：

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220606105129824.png" alt="image-20220606105129824" style="zoom:50%;" />

​		

## 12.2 Mybatis-Spring - 方式一

> 传送门：[mybatis-spring中文文档](https://mybatis.org/spring/zh/ "点击查看mybatis-Spring帮助文档")

在开始使用 MyBatis-Spring 之前，你需要先熟悉 Spring 和 MyBatis 这两个框架和有关它们的术语。这很重要——因为本手册中不会提供二者的基本内容，安装和配置教程。

MyBatis-Spring 需要以下版本：

| MyBatis-Spring | MyBatis | Spring Framework | Spring Batch | Java    |
| :------------- | :------ | :--------------- | :----------- | :------ |
| **2.0**        | 3.5+    | 5.0+             | 4.0+         | Java 8+ |
| **1.3**        | 3.4+    | 3.2.2+           | 2.1+         | Java 6+ |

如果使用 Maven 作为构建工具，仅需要在 pom.xml 中加入以下代码即可：

```xml
<dependency>
   <groupId>org.mybatis</groupId>
   <artifactId>mybatis-spring</artifactId>
   <version>2.0.2</version>
</dependency>
```

**整合**【pom.xml】（<font color="green">**模板使用**</font>）

```xml
<dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.46</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>5.3.19</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.1.10.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.1.10.RELEASE</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.4</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.2</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.22</version>
        </dependency>
    </dependencies>

    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
```

​	

1. 引入Spring配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/aop
          http://www.springframework.org/schema/aop/spring-aop.xsd">
   ```

2. 配置数据源替换mybaits的数据源

   ```xml
    <!--DataSource:使用Spring的数据源替换mybatis的配置    c3p0  dbcp
       这里使用Spring提供的jdbc-->
       <bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
           <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
           <property name="url"
                     value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF8"/>
           <property name="username" value="root"/>
           <property name="password" value="123456"/>
       </bean>
   ```

3. SqlSessionFactory，关联MyBatis

   ```xml
   <!--sqlSessionFactory-->
       <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
           <property name="dataSource" ref="datasource"/>
   
           <!--绑定mybatis配置文件-->
           <property name="configLocation" value="classpath:Mybatis-config.xml"/>
           <property name="mapperLocations" value="classpath:com/xleixz/mapper/*.xml"/>
       </bean>
   ```

4. SqlSessionTemplate，关联sqlSessionFactory；

   ```xml
   <!--SqlSessionTemplate就是我们使用的SqlSession-->
       <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
           <!--只能使用构造器注入SqlSessionFactory，因为它没有set方法-->
           <constructor-arg index="0" ref="sqlSessionFactory"/>
       </bean>
   ```

   ​	

   **整合Spring-dao（Spring配置mybatis）配置文件**【Spring-dao】（<font color="green">**模板使用**</font>）

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/aop
          http://www.springframework.org/schema/aop/spring-aop.xsd">
       
        <!--DataSource:使用Spring的数据源替换mybatis的配置    c3p0  dbcp
       这里使用Spring提供的jdbc-->
       <bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
           <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
           <property name="url"
                     value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF8"/>
           <property name="username" value="root"/>
           <property name="password" value="123456"/>
       </bean>
       
       <!--sqlSessionFactory-->
       <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
           <property name="dataSource" ref="datasource"/>
   
           <!--绑定mybatis配置文件-->
           <property name="configLocation" value="classpath:Mybatis-config.xml"/>
           <property name="mapperLocations" value="classpath:com/xleixz/mapper/*.xml"/>
       </bean>
       
       <!--SqlSessionTemplate就是mybatis使用的SqlSession-->
       <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
           <!--只能使用构造器注入SqlSessionFactory，因为它没有set方法-->
           <constructor-arg index="0" ref="sqlSessionFactory"/>
       </bean>
     
   </beans>
   ```

5. 需要给接口加实现类

   ```java
   public class UserMapperImpl implements UserMapper {
   
       //我们以前的所有操作都使用SqlSession来执行，
       //我们现在都是用SqlSessionTemplate来执行
       private SqlSessionTemplate sqlSession;
   
       public void setSqlSession(SqlSessionTemplate sqlSession) {
           this.sqlSession = sqlSession;
       }
   
       @Override
       public List<User> selectUser() {
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
           return mapper.selectUser();
       }
   }
   ```

6. 注册bean实现

   ```xml
   <!--实现类注册-->
       <bean id="userMapper" class="com.xleixz.mapper.UserMapperImpl">
           <property name="sqlSession" ref="sqlSession"/>
       </bean>
   ```

7. 测试类

   ```java
   public class MyTest {
   
       @Test
       public void test() {
   
           ApplicationContext context = new ClassPathXmlApplicationContext("ApplicationContext.xml");
   
           UserMapper userMapper = context.getBean("userMapper", UserMapper.class);
   
           for (User user : userMapper.selectUser()) {
               System.out.println(user);
           }
       }
   }
   ```

目录结构：

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220606150849294.png" alt="image-20220606150849294" style="zoom: 50%;" />

​	

## 12.3 Mybatis-Spring - 方式二【了解】

1. 将上面写的UserDaoImpl修改一下，继承`SqlSessionDaoSupport`即可

   ```java
   public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper {
   
       @Override
       public List<User> selectUser() {
           SqlSession sqlSession = getSqlSession();
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
           return getSqlSession().getMapper(UserMapper.class).selectUser();
       }
   }
   ```

2. 注册bean

   ```xml
   <bean id="userMapper2" class="com.xleixz.mapper.UserMapperImpl2">
          <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
   </bean>
   ```

3. 测试

   ```java
   public class MyTest {
   
       @Test
       public void test() {
   
           ApplicationContext context = new ClassPathXmlApplicationContext("ApplicationContext.xml");
   
           UserMapperImpl userMapper = context.getBean("userMapper", UserMapperImpl.class);
   
           for (User user : userMapper.selectUser()) {
               System.out.println(user);
           }
       }
   }
   ```

这种方式简洁了很多操作，了解即可。

---

​	

# 13、声明式事务

- 把一组业务当成一个业务来做，要么都成功，要么都失败！
- 事务在项目开发中，十分的重要，涉及到数据的一致性问题！
- 确保完整性和一致性

​	

事务的ACID原则：

- 原子性：一个事务（transaction）中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环

  节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过

  一样。

- 一致性：在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的资料必须完全符合所有

  的预设规则，这包含资料的精确度、串联性以及后续数据库可以自发性地完成预定的工作。

- 隔离性：数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时

  由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括读未提交（Read uncommitted）、读提交（read committed）、可重复读（repeatable read）和串行化（Serializable），防止数据损坏。

- 持久性：事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。

​		

## 13.1 Spring中的事务管理

Spring在不同的事务管理API之上定义了一个抽象层，使得开发人员不必了解底层的事务管理API就可以使用Spring

的事务管理机制。Spring支持编程式事务管理和声明式的事务管理。

**编程式事务管理**

- 将事务管理代码嵌到业务方法中来控制事务的提交和回滚
- 缺点：必须在每个事务操作业务逻辑中包含额外的事务管理代码

**声明式事务管理**【交由容器管理事务】

- 一般情况下比编程式事务好用。
- 将事务管理代码从业务方法中分离出来，以声明的方式来实现事务管理。
- 将事务管理作为横切关注点，通过aop方法模块化。Spring中通过Spring AOP框架支持声明式事务管理。

​	

**使用Spring管理事务，注意头文件的约束导入 : tx**

```xml
xmlns:tx="http://www.springframework.org/schema/tx"

http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd">
```

**事务管理器**

- 无论使用Spring的哪种事务管理策略（编程式或者声明式）事务管理器都是必须的。
- 就是 Spring的核心事务管理抽象，管理封装了一组独立于技术的方法。

**JDBC事务**

```xml
<!--配置声明式事务-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
       <property name="dataSource" ref="dataSource" />
</bean>
```

**配置好事务管理器后需要配置事务的通知**

```xml
<!--配置事务通知-->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
   <tx:attributes>
       <!--配置哪些方法使用什么样的事务,配置事务的传播特性-->
       <tx:method name="add" propagation="REQUIRED"/>
       <tx:method name="delete" propagation="REQUIRED"/>
       <tx:method name="update" propagation="REQUIRED"/>
       <tx:method name="search*" propagation="REQUIRED"/>
       <tx:method name="get" read-only="true"/>
       <tx:method name="*" propagation="REQUIRED"/>
   </tx:attributes>
</tx:advice>
```

**spring事务传播特性：**

事务传播行为就是多个事务方法相互调用时，事务如何在这些方法间传播。spring支持7种事务传播行为：

- propagation_requierd：如果当前没有事务，就新建一个事务，如果已存在一个事务中，加入到这个事务中，这是最常见的选择。

- propagation_supports：支持当前事务，如果没有当前事务，就以非事务方法执行。

- propagation_mandatory：使用当前事务，如果没有当前事务，就抛出异常。

- propagation_required_new：新建事务，如果当前存在事务，把当前事务挂起。

- propagation_not_supported：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。

- propagation_never：以非事务方式执行操作，如果当前事务存在则抛出异常。

- propagation_nested：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与

  propagation_required类似的操作

Spring 默认的事务传播行为是 PROPAGATION_REQUIRED，它适合于绝大多数的情况。

假设 `ServiveX#methodX()` 都工作在事务环境下（即都被 Spring 事务增强了），假设程序中存在如下的调用链：

`Service1#method1()->Service2#method2()->Service3#method3()`，那么这 3 个服务类的 3 个方法通过 Spring 的事务

传播机制都工作在同一个事务中。

就好比，我们刚才的几个方法存在调用，所以会被放在一组事务当中！

**配置AOP**

导入aop的头文件！

```xml
<!--配置aop织入事务-->
<aop:config>
   <aop:pointcut id="txPointcut" expression="execution(* com.kuang.dao.*.*(..))"/>
   <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>
</aop:config>
```

**进行测试**

删掉刚才插入的数据，再次测试！

```java
@Test
public void test2(){
   ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
   UserMapper mapper = (UserMapper) context.getBean("userDao");
   List<User> user = mapper.selectUser();
   System.out.println(user);
}
```

​	

> 为什么需要配置事务？

- 如果不配置，就需要手动提交控制事务；
- 事务在项目开发过程非常重要，涉及到数据的一致性的问题！

---

​	

# 14、项目：Mybatis-Spring【模板】

1. 在【pom.xml】文件中**导入依赖jar包**和**配置Maven静态资源过滤**

   ```xml
   <dependencies>
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
               <scope>test</scope>
           </dependency>
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>5.1.46</version>
           </dependency>
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis</artifactId>
               <version>3.5.2</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-beans</artifactId>
               <version>5.3.19</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>5.1.10.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-jdbc</artifactId>
               <version>5.1.10.RELEASE</version>
           </dependency>
   
           <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
           <dependency>
               <groupId>org.aspectj</groupId>
               <artifactId>aspectjweaver</artifactId>
               <version>1.9.4</version>
           </dependency>
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis-spring</artifactId>
               <version>2.0.2</version>
           </dependency>
           <dependency>
               <groupId>log4j</groupId>
               <artifactId>log4j</artifactId>
               <version>1.2.17</version>
           </dependency>
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <version>1.18.22</version>
           </dependency>
   </dependencies>
   
   <build>
           <resources>
               <resource>
                   <directory>src/main/java</directory>
                   <includes>
                       <include>**/*.properties</include>
                       <include>**/*.xml</include>
                   </includes>
                   <filtering>true</filtering>
               </resource>
           </resources>
   </build>
   ```

2. 在**pojo文件夹**中创建**实体类**【Family.java】

   ```java
   //实体类
   public class Family {
   
       private String name;
       private String sex;
       private int age;
   
       public Family(String name, String sex, int age) {
           this.name = name;
           this.sex = sex;
           this.age = age;
       }
   
       public Family() {
       }
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       public String getSex() {
           return sex;
       }
   
       public void setSex(String sex) {
           this.sex = sex;
       }
   
       public int getAge() {
           return age;
       }
   
       public void setAge(int age) {
           this.age = age;
       }
   
       @Override
       public String toString() {
           return "Family{" +
                   ", name='" + name + '\'' +
                   ", sex='" + sex + '\'' +
                   ", age=" + age +
                   '}';
       }
   }
   ```

3. 在**resources文件夹中**创建**spring配置mybatis文件**【Spring-dao.xml】，为了连接数据库等操作

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/aop
          http://www.springframework.org/schema/aop/spring-aop.xsd">
   
       <!--DataSource:使用Spring的数据源替换mybatis的配置    c3p0  dbcp
      这里使用Spring提供的jdbc-->
       <bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
           <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
           <property name="url"
                     value="jdbc:mysql://localhost:3306/family_spring_mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF8"/>
           <property name="username" value="root"/>
           <property name="password" value="123456"/>
       </bean>
   
       <!--sqlSessionFactory-->
       <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
           <property name="dataSource" ref="datasource"/>
   
           <!--绑定mybatis配置文件-->
           <property name="configLocation" value="classpath:Mybatis-config.xml"/>
           <property name="mapperLocations" value="classpath:com/xleixz/mapper/*.xml"/>
       </bean>
   
       <!--SqlSessionTemplate就是mybatis使用的SqlSession-->
       <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
           <!--只能使用构造器注入SqlSessionFactory，因为它没有set方法-->
           <constructor-arg index="0" ref="sqlSessionFactory"/>
       </bean>
   
   </beans>
   ```

4. 在**resources文件夹中**创建**mybatis核心配置文件**【Mybatis-config.xml】，为了表示mybatis的存在，可以在这里设置起别名和设置等一些配置

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
   
       <!--alias起别名-->
       <typeAliases>
           <package name="com.xleixz.pojo"/>
       </typeAliases>
   
   </configuration>
   ```

5. 在**resources文件夹**中创建**spring配置文件**【ApplicationContext.xml】，用于注册代理，接口实现类bean

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/aop
          http://www.springframework.org/schema/aop/spring-aop.xsd">
       
   </beans>
   ```

6. 在**mapper文件夹**中创建**实体类接口**【FamilyMapper.java】

   ```java
   //实体类接口
   public interface FamilyMapper {
   
       //查询用户
       public Family selectFamily(int id);
   
       //增加用户
       public int addFamily(Family family);
   
       //修改用户
       public int updateFamily(Family family);
   
       //删除用户
       public int deleteFamily(int id);
   }
   ```

7. 在**mapper文件夹中**创建**实体类接口对应的SQL方法**mybatis配置文件【FamilyMapper.xml】（<font color="red">**注意**</font>：这里尽量不要有中文注释，会报`UTF-8 序列的字节无效`错误）

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   
   <!--mybatis实现SQL语句-->
   
   <!--namespace绑定接口FamilyMapper-->
   <mapper namespace="com.xleixz.mapper.FamilyMapper">
   
   
       <select id="selectFamily" parameterType="int" resultType="family">
           select *
           from family
           where id = #{id}
       </select>
   
       <!--parameterType: 传参类型,接口中的方法为引用数据类型Family,这里就是传入的参数类型就是Family,
           在Mybatis-config.xml中配置了typeAliases起别名,所以为family,增改查不需要查看所以无需返回值-->
       <insert id="addFamily" parameterType="family">
           insert into family(name, sex, age)
           values (#{name}, #{sex}, #{age});
       </insert>
   
       <!--parameterType: 传参类型,接口中的方法为引用数据类型Family,这里就是传入的参数类型就是Family,
           在Mybatis-config.xml中配置了typeAliases起别名,所以为family,增改查不需要查看所以无需返回值-->
       <update id="updateFamily" parameterType="family">
           update family
           set sex=#{sex},age=#{age}
           where name= #{name}
       </update>
   
       <!--parameterType: 传参类型,接口中的方法引用的为int类型,这里就是传入的参数类型就是int;增改查不需要查看所以无需返回值-->
       <delete id="deleteFamily" parameterType="int">
           delete
           from family
           where id = #{id}
       </delete>
   
   </mapper>
   ```

8. 在**mapper文件夹**中创建**接口实现类**【FamilyImpl】，代理模式中的代理角色（中介）

   ```java
   //接口实现类，代理模式中的代理角色（中介）
   public class FamilyImpl implements FamilyMapper {
       //注入SqlSessionTemplate对象
       private SqlSessionTemplate sqlSessionTemplate;
   
       //set方法注入
       public void setSqlSessionTemplate(SqlSessionTemplate sqlSessionTemplate) {
           this.sqlSessionTemplate = sqlSessionTemplate;
       }
   
       //实现查询
       @Override
       public Family selectFamily(int id) {
           //调用sqlSessionTemplate对象的getMapper方法,获取mapper接口的实现类对象
           FamilyMapper familyMapper = sqlSessionTemplate.getMapper(FamilyMapper.class);
           //调用mapper接口的方法,传递参数,获取返回值
           return familyMapper.selectFamily(id);
       }
   
       //实现添加用户
       @Override
       public int addFamily(Family family) {
           //调用sqlSessionTemplate对象的getMapper方法,获取mapper接口的实现类对象
           FamilyMapper familyMapper = sqlSessionTemplate.getMapper(FamilyMapper.class);
           //调用mapper接口的方法,传递参数,获取返回值
           return familyMapper.addFamily(family);
       }
   
       //实现修改用户
       @Override
       public int updateFamily(Family family) {
           //调用sqlSessionTemplate对象的getMapper方法,获取mapper接口的实现类对象
           FamilyMapper familyMapper = sqlSessionTemplate.getMapper(FamilyMapper.class);
           //调用mapper接口的方法,传递参数,获取返回值
           return familyMapper.updateFamily(family);
       }
   
       //实现删除用户
       @Override
       public int deleteFamily(int id){
           //调用sqlSessionTemplate对象的getMapper方法,获取mapper接口的实现类对象
           FamilyMapper familyMapper = sqlSessionTemplate.getMapper(FamilyMapper.class);
           //调用mapper接口的方法,传递参数,获取返回值
           return familyMapper.deleteFamily(id);
       }
   }
   ```

9. 在第5步中的**spring配置文件**【ApplicationContext.xml】中**注册bean绑定接口实现类**

   ```xml
   <!--注册代理，接口实现类bean-->
   <import resource="spring-dao.xml"/>
   
       <bean id="FamilyImpl" class="com.xleixz.mapper.FamilyImpl">
           <!--这里的name对应的是FamilyImpl接口中的set方法后的值,ref对应的是Spring-dao配置文件中创建的id:SqlSessionTemplate-->
           <property name="sqlSessionTemplate" ref="sqlSession"/>
   </bean>
   ```

   ![image-20220609101700666](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220609101700666.png)

10. 在**Test文件夹**中创建**测试类**【MyTest.java】

    ```java
    //测试类
    public class MyTest {
    
        //查询用户
        @Test
        public void select() {
            //创建spring容器(ApplicationContext(上下文)对象),加载spring配置文件
            ApplicationContext applicationContext = new ClassPathXmlApplicationContext("ApplicationContext.xml");
            //获取对象,注意这里是实现类的名称,UserMapperImpl,这里的名称要与spring配置文件中的bean id一致
            FamilyImpl familyImpl = applicationContext.getBean("FamilyImpl", FamilyImpl.class);
            //调用方法,传入参数
            Family family = familyImpl.selectFamily(1);
    
            System.out.println(family);
    
        }
    
        //添加用户
        @Test
        public void add() {
            //创建spring容器(ApplicationContext(上下文)对象),加载spring配置文件
            ApplicationContext applicationContext = new ClassPathXmlApplicationContext("ApplicationContext.xml");
            //获取对象,注意这里是实现类的名称,UserMapperImpl,这里的名称要与spring配置文件中的bean id一致
            FamilyImpl familyImpl = applicationContext.getBean("FamilyImpl", FamilyImpl.class);
            //创建Family对象
            Family family = new Family("小张", "女", 22);
            Family family2 = new Family("小江", "男", 22);
            //调用方法,传递参数,这里的参数是实体类
            familyImpl.addFamily(family);
        }
    
        //修改用户
        @Test
        public void update() {
            //创建spring容器(ApplicationContext(上下文)对象),加载spring配置文件
            ApplicationContext context = new ClassPathXmlApplicationContext("ApplicationContext.xml");
            //获取对象,注意这里是实现类的名称,UserMapperImpl,这里的名称要与spring配置文件中的bean id一致
            FamilyImpl familyImpl = context.getBean("FamilyImpl", FamilyImpl.class);
            //创建Family对象,这里的参数是实体类
            Family family = new Family("小雷","男",25);
    
            familyImpl.updateFamily(family);
        }
    
        //删除用户
        @Test
        public void delete(){
            //创建spring容器(ApplicationContext(上下文)对象),加载spring配置文件
            ApplicationContext context = new ClassPathXmlApplicationContext("ApplicationContext.xml");
            //获取对象,注意这里是实现类的名称,UserMapperImpl,这里的名称要与spring配置文件中的bean id一致
            FamilyImpl familyImpl = context.getBean("FamilyImpl",FamilyImpl.class);
            //调用方法,传递参数
            familyImpl.deleteFamily(3);
    
        }
    }
    ```

​	

结构：

![image-20220609102323011](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220609102323011.png)

​		

​		

<font color="green>">**完结！  2022-06-06**</font>
