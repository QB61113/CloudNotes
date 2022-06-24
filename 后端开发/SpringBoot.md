# SpringBoot

# 1 SpringBoot简介

> **回顾Spring**

Spring是一个开源框架，2003 年兴起的一个轻量级的Java 开发框架，作者：Rod Johnson 。

**Spring是为了解决企业级应用开发的复杂性而创建的，简化开发。**

​	

> **Spring**

为了降低Java开发的复杂性，Spring采用了以下4种关键策略：

1、基于POJO的轻量级和最小侵入性（非入侵式）编程，所有东西都是bean；

2、通过IOC，依赖注入（DI）和面向接口实现松耦合；

3、基于切面（AOP）和惯例进行声明式编程；

4、通过切面和模版减少样式代码，`RedisTemplate`，`xxxTemplate`；

​	

> **什么是SpringBoot**

在Javaweb时，最开始是用Servlet结合Tomcat，跑出一个Hello World程序！是要经历特别多的步骤，后来到了后来

就用了框架Struts，再后来是SpringMVC，到了现在的SpringBoot，过一两年又会有其他web框架出现；

**SpringBoot**，就是一个javaweb的开发框架，和SpringMVC类似，对比其他javaweb框架的好处，官方说是简化开

发，**约定大于配置**， 能迅速的开发web应用，几行代码开发一个http接口。

Spring Boot 以<font color="red">**约定大于配置的核心思想**</font>，默认帮我们进行了很多设置，多数 Spring Boot 应用只需要很少的 Spring

 配置。同时它集成了大量常用的第三方库配置（例如 Redis、MongoDB、Jpa、RabbitMQ、Quartz 等等），Spring

Boot 应用中这些第三方库几乎可以零配置的开箱即用。

简单来说就是SpringBoot其实不是什么新的框架，它默认配置了很多框架的使用方式，就像maven整合了所有的jar

包，spring boot整合了所有的框架 。Spring Boot 出生名门，从一开始就站在一个比较高的起点，又经过这几年的

发展，生态足够完善，Spring Boot 已经当之无愧成为 Java 领域最热门的技术。

​	

**Spring Boot的主要优点：**

- 为所有Spring开发者更快的入门
- **开箱即用**，提供各种默认配置来简化项目配置
- 内嵌式容器简化Web项目
- 没有冗余代码生成和XML配置的要求

---

​	

# 2 第一个SpringBoot程序

## 2.1 Hello SpringBoot

> **Hello，World**

环境准备：

- jdk 1.8
- Maven 3.8.1
- SpringBoot 2.x 最新版

开发工具：

- IDEA

​	

> **创建基础项目说明**

Spring官方提供了非常方便的工具让我们快速构建应用。

传送门：[Spring Initializr 快速构建应用](https://start.spring.io/ "点击快速构建应用")

![image-20220618173850685](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220618173850685.png)



**创建方式一：**使用Spring Initializr 的 Web页面创建项目

1、打开  [https://start.spring.io/](https://start.spring.io/)

2、填写项目信息

3、点击`Generate Project`按钮生成项目；下载此项目

4、解压项目包，并用IDEA以Maven项目导入，一路下一步即可，直到项目导入完毕。

5、如果是第一次使用，可能速度会比较慢，包比较多、需要耐心等待一切就绪。

​	

**创建方式二：**直接在IDEA中创建项目

1、创建一个新项目

2、选择`spring initalizr`， 可以看到默认就是去官网的快速构建工具那里实现

3、填写项目信息

4、选择初始化的组件（初学勾选 Web 即可）

5、填写项目路径

6、等待项目构建成功

​	

**项目结构分析：**

通过上面步骤完成了基础项目的创建。就会自动生成以下文件。

1、程序的主启动类`HelloWorldApplication`，<font color="green">**他是程序的主入口，不能删也不能改！！！**</font>

2、一个 `application.properties` 配置文件，<font color="green">**他是SpringBoot的核心配置文件**</font>

3、一个 单元测试类`HelloWorldApplicationTests`

4、一个 pom.xml

![image-20220618182750159](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220618182750159.png)

​	

> **分析一下pom.xml**

```xml
<!-- 父依赖 -->
<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.7.0</version>
		<relativePath/> <!-- lookup parent from repository -->
</parent>

<dependencies>
        <!-- web场景启动器, web依赖: Tomcat, DispatcherServlet, xml....... -->-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<!-- springboot单元测试 -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
             <!-- 剔除依赖 -->
             <exclusions>
             <exclusion>
                <groupId>org.junit.vintage</groupId>
                <artifactId>junit-vintage-engine</artifactId>
            </exclusion>
        </exclusions>
		</dependency>
	</dependencies>

	<build>
		<plugins>
            <!-- 打包插件 -->
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
```

​	

> **编写一个Http接口**

1、在**主程序的同级目录下**，新建一个`controller`包，<font color="red">**一定要在主程序同级目录下，否则识别不到**</font>

![image-20220618183731285](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220618183731285.png)

2、在包中新建一个`HelloController`类，用于调用业务，接收前端参数

```java
//本身就是一个Spring组件

@RestController
public class HelloController {

    //用于调用业务，接收前端参数
    @RequestMapping("/hello")
    public String hello() {
        return "Hello World!";
    }
}
```

3、编写完毕后，从主程序启动项目`HelloWorldApplication`，浏览器发起请求，看页面返回；控制台输出了 Tomcat 访问的端口号！

![image-20220618184054478](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220618184054478.png)

​	

> **将项目打成jar包**

:mag:点击Maven的`Lifecycle`下的`package`

如果打包成功，则会在target目录下生成一个 jar 包，就可以在任何地方运行了！

如果遇到错误，可以配置打包时跳过项目运行测试用例。

```xml
<!--
    在工作中,很多情况下我们打包是不想执行测试用例的
    可能是测试用例不完事,或是测试用例会影响数据库数据
    跳过测试用例执
    -->
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <!--跳过项目运行测试用例-->
        <skipTests>true</skipTests>
    </configuration>
</plugin>
```

​	

<font color="green" >**所以SpringBoot的核心原理是：自动装配**</font>

​	

## 2.2 修改端口号

在**resources目录下**的**application.properties**添加：

```properties
#更改项目端口号
server.port=8081
```

![image-20220618192330997](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220618192330997.png)

​	

## 2.3 修改Banner

在项目下的 **resources 目录**下新建一个**banner.txt** 即可。

图案可以到：[banner图案](https://www.bootschool.net/ascii "点击到banner图案网站") 这个网站生成，然后拷贝到文件中即可！

---

​	

# 3 自动装配运行原理（初期理解）

## 3.1 分析【pom.xml】

:mag:spring-boot-dependencies：核心依赖在父工程中，所有导入依赖时不需要写版本号，有版本仓库！

> **启动器**

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter</artifactId>
</dependency>
```

:mag:这就是SpringBoot的启动场景；

:mag:比如spring-boot-starter-web，他会自动导入web环境所有的依赖；

:mag:SpringBoot会将所有的功能场景，都一个一个变成启动器

:mag:需要使用什么功能，就只需要对应的启动器就可以了`starter`

​	

## 3.2 分析主程序

> **分析【SpringBootStudyApplication.java】主程序**

```java
@SpringBootApplication
public class SpringBootStudyApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBootStudyApplication.class, args);
    }
}
```

:mag:`@SpringBootApplication`：标注这个类是一个**SpringBoot**的应用；

:mag:`SpringApplication.run(SpringBootStudyApplication.class, args);`：将SpringBoot应用启动；

​	

**注解**

:mag:`@SpringBootConfiguration`：**SpringBoot的配置**

:mag:`@Configuration`：**Spring配置类**

:mag:`@Component`：**也是一个Spring的组件**

:mag:`@EnableAutoConfiguration`：**自动配置**

:mag:`@AutoConfigurationPackage`：**自动配置包**

:mag:`@Import(AutoConfigurationPackage.Registrar.class)`**导入自动配置`包注册`**

:mag:`@Import(AutoConfigurationSelector.class)`**自动配置导入选择**

​	

**结论：**

1. SpringBoot在启动的时候从类路径下的`META-INF/spring.factories`中获取`EnableAutoConfiguration`指定的值

2. 将这些值作为自动配置类导入容器 ， 自动配置类就生效 ， 帮我们进行自动配置工作；

3. 整个J2EE的整体解决方案和自动配置都在`springboot-autoconfigure`的**jar**包中；

4. 它会给容器中导入非常多的自动配置类 （xxxAutoConfiguration）, 就是给容器中导入这个场景需要的所有组

   件 ， 并配置好这些组件 ；

5. 有了自动配置类 ， 免去了我们手动编写配置注入功能组件等的工作；

​		

## 3.3 主启动类运行原理

> **SpringBootStudyApplication类**

```java
@SpringBootApplication
public class SpringBootStudyApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBootStudyApplication.class, args);
    }

}
```

<font color="green">**他不仅运行了一个main方法，而且还开启了一个服务！**</font>

​	

**SpringApplication.run分析**

分析该方法主要分两部分，一部分是SpringApplication的实例化，二是run方法的执行；

**这个类主要做了以下四件事情：**

1、推断应用的类型是普通的项目还是Web项目

2、查找并加载所有可用初始化器 ， 设置到initializers属性中

3、找出所有的应用程序监听器，设置到listeners属性中

4、推断并设置main方法的定义类，找到运行的主类

查看构造器：

```java
public SpringApplication(ResourceLoader resourceLoader, Class... primarySources) {
    // ......
    this.webApplicationType = WebApplicationType.deduceFromClasspath();
    this.setInitializers(this.getSpringFactoriesInstances();
    this.setListeners(this.getSpringFactoriesInstances(ApplicationListener.class));
    this.mainApplicationClass = this.deduceMainApplicationClass();
}
```

​	

> **run方法**

流程分析：

![image-20220619104943116](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220619104943116.png)

---

​	

# 4 yaml配置注入

## 4.1 yaml简介

> **配置文件**

SpringBoot使用一个全局的配置文件 ， 配置文件名称是<font color="red">**固定的**</font>，**官网建议使用yml配置文件**。

- **application.properties**

- - 语法结构 ：key=value

- **application.yml**

- - 语法结构 ：key：空格 value

**配置文件的作用 ：**修改SpringBoot自动配置的默认值，因为SpringBoot在底层都给自动配置好了；

比如可以在配置文件**properties**中修改Tomcat 默认启动的端口号！

```properties
server.port=8081
```

**而在application.yml**配置文件中，修改Tomcat默认启动的端口号！

```yaml
server:
  port: 8001
```

![image-20220619111238107](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220619111238107.png)

​	

> **yaml简介**

YAML是 "YAML Ain't a Markup Language" （YAML不是一种标记语言）的递归缩写。在开发的这种语言时，YAML 的

意思其实是："Yet Another Markup Language"（仍是一种标记语言）

**这种语言以数据作为中心，而不是以标记语言为重点！**

以前的配置文件，大多数都是使用xml来配置；比如一个简单的端口配置，对比下yaml和xml

传统xml配置：

```xml
<server>
    <port>8081<port>
</server>
```

yaml配置：

```yaml
server：
  prot: 8080
```

​	

## 4.2 yaml基本语法

> <font color="red">**说明：语法要求严格！**</font>
>
> 1、空格不能省略
>
> 2、以缩进来控制层级关系，只要是左边对齐的一列数据都是同一个层级的。
>
> 3、属性和值的大小写都是十分敏感的。

​	

> **基本语法**

**字面量：普通的值  [ 数字，布尔值，字符串  ]**

:mag:字面量直接写在后面，字符串默认**不用加上双引号或者单引号；**

<font color="red">**注意：**</font>

`""` 双引号，**不会转义字符串里面的特殊字符** ， 特殊字符会作为本身想表示的意思；

```yaml
比如 ：name: "kuang \n shen"  输出 ：kuang  换行  shen
```

`''` 单引号，**会转义特殊字符** ， 特殊字符最终会变成和普通字符一样输出

```yaml
比如 ：name: ‘kuang \n shen’  输出 ：kuang  \n  shen
```

![image-20220619112308143](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220619112308143.png)

​	

> **存对象**

```yaml
name: xleixz

student:
  name: xleixz
  age: 22

# 行内写法
student2: {name: xleixz,age: 22}
```

​		

> **数组**

```yaml
pets:
    - cat
    - dog
    - pig

# 行内写法
pets2: [cat,dog,pig]
```

​	

## 4.3 yaml注入配置文件

> **yaml可以直接给实体类赋值**

1、准备一个实体类【Dog.java】

```java
//注册bean  等价于 <bean id="dog" class="com.xleixz.pojo.Dog"/>
@Component
public class Dog {

    private String name;
    private int age;

    public Dog() {
    }

    public Dog(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

2、使用原本的`@value`注解绑定值

```java
@Component //注册bean
public class Dog {
    @Value("旺柴")
    private String name;
    @Value("2")
    private Integer age;
}
```

3、在SpringBoot的测试类下注入狗狗输出一下

```java

@SpringBootTest
class DemoApplicationTests {

    @Autowired //将狗狗自动注入进来
    Dog dog;

    @Test
    public void contextLoads() {
        System.out.println(dog); //打印看下狗狗对象
    }

}
```

4、结果

![image-20220621001604492](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621001604492.png)

<font color="red">**但是凭借@value注解一直设置值显然是不可取的，如果属性较多，则会变得繁琐**</font>

​	

<font color="green">**解决：通过yaml配置文件注入**</font>

1、准备一个实体类【Person.java】

```java
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String name;
    private int age;
    private Boolean happy;
    private Date brithday;
    private Map<String, Object> maps;
    private List<Object> lists;
    private Dog dog;

    //有参、无参、getter、setter、toString
}
```

2、通过yaml配置文件的方式注入到类中

```yaml
person:
  name: xleixz
  age: 22
  happy: false
  brithday: 2022/01/01
  maps: { k1: v1,k2: v2 }
  lists: [1,2,3]
  dog:
    name: 旺柴
    age: 2
```

3、**使用`@ConfigurationProperties(prefix="person")`注解将配置文件中配置的属性映射到组件中，将配置文件中的person下面的所有属性一一对应**

![image-20220620234710453](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220620234710453.png)

```java
/*
@ConfigurationProperties作用：
将配置文件中配置的每一个属性的值，映射到这个组件中；
告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定
参数 prefix = “person” : 将配置文件中的person下面的所有属性一一对应
*/
@Component //注册bean
@ConfigurationProperties(prefix = "person")
public class Person {
    private String name;
    private Integer age;
    private Boolean happy;
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
}
```

4、<font color="red">IDEA 提示，SpringBoot配置注解处理器没有找到，导入一个pom依赖即可解决！</font>

![image-20220620234458605](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220620234458605.png)

```xml
<!-- 导入配置文件处理器，配置文件进行绑定就会有提示，需要重启 -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-configuration-processor</artifactId>
  <optional>true</optional>
</dependency>
```

5、测试一下

```java
@SpringBootTest
class DemoApplicationTests {

    @Autowired
    Person person; //将person自动注入进来

    @Test
    public void contextLoads() {
        System.out.println(person); //打印person信息
    }

}
```

![image-20220621002754523](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621002754523.png)

​	

> **yaml配置文件占位符**

配置文件还可以编写占位符生成随机数

```yaml
person:
    name: xleixz${random.uuid} # 随机uuid
    age: ${random.int}  # 随机int
    happy: false
    birth: 2022/01/01
    maps: {k1: v1,k2: v2}
    lists:
      - code
      - girl
      - music
    dog:
      name: ${person.hello:other}_旺财
      age: 1
```

​	

## 4.4 回顾不推荐的旧方法properties

上面采用的yaml方法都是最简单的方式，开发中最常用的；也是springboot所推荐的！配置文件除了yml还有我之

前常用的properties。

<font color="red">**注意：**在IDEA中编写properties配置文件时，会遇到编码格式问题，要改成UTF-8</font>

<font color="green">**解决：**settings-->FileEncodings 中配置</font>

![image-20220621003453511](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621003453511.png)

1. 新建一个实体类【User.java】

   ```java
   @Component //注册bean
   public class User {
       private String name;
       private int age;
       private String sex;
   }
   ```

2. 编辑配置文件【user.properties】

   ```properties
   user1.name=kuangshen
   user1.age=18
   user1.sex=男
   ```

3. 在User类上使用@Value来进行注入！

   ```java
   @Component //注册bean
   @PropertySource(value = "classpath:user.properties")
   public class User {
       //直接使用@value
       @Value("${user.name}") //从配置文件中取值
       private String name;
       @Value("#{9*2}")  // #{SPEL} Spring表达式
       private int age;
       @Value("男")  // 字面量
       private String sex;
   }
   ```

4. Springboot测试

   ```java
   @SpringBootTest
   class DemoApplicationTests {
   
       @Autowired
       User user;
   
       @Test
       public void contextLoads() {
           System.out.println(user);
       }
   
   }
   ```

5. 结果

   ![image-20220621003656293](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621003656293.png)

​	

> **结论**

:mag:**配置yml**和**配置properties**都可以获取到值 ， <font color="red"> **强烈推荐yml**；</font>

:mag:如果在某个业务中，只需要获取配置文件中的某个值，可以使用一下 `@value`；

:mag:如果说专门编写了一个JavaBean来和配置文件进行一一映射，就直接`@configurationProperties`【<font color="green">**最佳方案**</font>】！

----

​	

# 5 JSR303数据校验及多环境配置与切换

## 5.1 JSR303数据校验

SPringBoot可以通过`@Validated`注解来设置格式；**使用数据校验，可以保证数据的正确性。** 

1. 在pom中导入依赖

   ```xml
   <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-validation</artifactId>
   </dependency>
   ```

2. 在实体类中添加注解，以邮箱格式为例`@Email`

   ```java
   @Component
   @ConfigurationProperties(prefix = "person")
   
   //数据校验
   @Validated
   public class Person {
   
       @Email(message = "邮箱格式不正确")
       private String name;
       private int age;
       private Boolean happy;
       private Date brithday;
       private Map<String, Object> maps;
       private List<Object> lists;
       private Dog dog;
   
       //有参构造、无参构造、getter、setter、toString
   }
   ```

3. yaml配置文件中设置的值不为邮箱格式时

   ```yaml
   person:
     name: xleixz
   ```

4. 在测试类中测试

   ```java
   @SpringBootTest
   class SpringBoot02ConfigApplicationTests {
   
       @Autowired
       private Person person;
   
       @Test
       void contextLoads() {
           System.out.println(person);
       }
   
   }
   ```

5. 启动测试，提示：

   ![image-20220621101353542](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621101353542.png)

​	

>**常见的参数**

```markdown
@NotNull(message="名字不能为空")
private String userName;
@Max(value=120,message="年龄最大不能查过120")
private int age;
@Email(message="邮箱格式错误")
private String email;

空检查
@Null       验证对象是否为null
@NotNull    验证对象是否不为null, 无法查检长度为0的字符串
@NotBlank   检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格.
@NotEmpty   检查约束元素是否为NULL或者是EMPTY.
    
Booelan检查
@AssertTrue     验证 Boolean 对象是否为 true  
@AssertFalse    验证 Boolean 对象是否为 false  
    
长度检查
@Size(min=, max=) 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内  
@Length(min=, max=) string is between min and max included.

日期检查
@Past       验证 Date 和 Calendar 对象是否在当前时间之前  
@Future     验证 Date 和 Calendar 对象是否在当前时间之后  
@Pattern    验证 String 对象是否符合正则表达式的规则

.......等等
除此以外，还可以自定义一些数据校验规则
```

​	

## 5.2 多环境配置与切换

> **多环境配置**

**yaml配置文件的位置可放于以下四个位置：**

![image-20220621103217227](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621103217227.png)

![image-20220621103524186](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621103524186.png)

​	

> **多环境切换**

**方式一：**通过优先级覆盖的方式来切换。

**方式二：**yaml配置文件和properties配置文件手动选择。

- <font color="red">**properties配置文件手动切换方式，太繁琐【不推荐】**</font>

  ```properties
  #SpringBoot的多环境配置，可以选择激活某个环境。
  spring.profiles.active=dev
  ```

  ![image-20220621104058714](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621104058714.png)

- <font color="green">**通过yaml配置文件多文档模块切换环境，【推荐】**</font>

  ```yaml
  server:
    port: 8081
  #选择要激活那个环境块
  spring:
    profiles:
      active: prod
  
  ---
  server:
    port: 8083
  spring:
    profiles: dev #配置环境的名称
  
  
  ---
  
  server:
    port: 8084
  spring:
    profiles: prod  #配置环境的名称
  ```

​	

<font color="red">**注意：如果yml和properties同时都配置了端口，并且没有激活其他环境 ， 默认会使用properties配置文件的！**</font>

---

​	

# 6 自动装配运行原理（终期理解）

==（非原创）==

> 配置文件到底能写什么？怎么写？

SpringBoot官方文档中有大量的配置，我们无法全部记住

![图片](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/640)

​	

以**HttpEncodingAutoConfiguration（Http编码自动配置）**为例解释自动配置原理；

```java
//表示这是一个配置类，和以前编写的配置文件一样，也可以给容器中添加组件；
@Configuration 

//启动指定类的ConfigurationProperties功能；
  //进入这个HttpProperties查看，将配置文件中对应的值和HttpProperties绑定起来；
  //并把HttpProperties加入到ioc容器中
@EnableConfigurationProperties({HttpProperties.class}) 

//Spring底层@Conditional注解
  //根据不同的条件判断，如果满足指定的条件，整个配置类里面的配置就会生效；
  //这里的意思就是判断当前应用是否是web应用，如果是，当前配置类生效
@ConditionalOnWebApplication(
    type = Type.SERVLET
)

//判断当前项目有没有这个类CharacterEncodingFilter；SpringMVC中进行乱码解决的过滤器；
@ConditionalOnClass({CharacterEncodingFilter.class})

//判断配置文件中是否存在某个配置：spring.http.encoding.enabled；
  //如果不存在，判断也是成立的
  //即使我们配置文件中不配置pring.http.encoding.enabled=true，也是默认生效的；
@ConditionalOnProperty(
    prefix = "spring.http.encoding",
    value = {"enabled"},
    matchIfMissing = true
)

public class HttpEncodingAutoConfiguration {
    //他已经和SpringBoot的配置文件映射了
    private final Encoding properties;
    //只有一个有参构造器的情况下，参数的值就会从容器中拿
    public HttpEncodingAutoConfiguration(HttpProperties properties) {
        this.properties = properties.getEncoding();
    }
    
    //给容器中添加一个组件，这个组件的某些值需要从properties中获取
    @Bean
    @ConditionalOnMissingBean //判断容器没有这个组件？
    public CharacterEncodingFilter characterEncodingFilter() {
        CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
        filter.setEncoding(this.properties.getCharset().name());
        filter.setForceRequestEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.REQUEST));
        filter.setForceResponseEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.RESPONSE));
        return filter;
    }
    //。。。。。。。
}
```

​	

**一句话总结 ：根据当前不同的条件判断，决定这个配置类是否生效！**

- 一但这个配置类生效；这个配置类就会给容器中添加各种组件；
- 这些组件的属性是从对应的properties类中获取的，这些类里面的每一个属性又是和配置文件绑定的；
- 所有在配置文件中能配置的属性都是在xxxxProperties类中封装着；
- 配置文件能配置什么就可以参照某个功能对应的这个属性类

```java

//从配置文件中获取指定的值和bean的属性进行绑定
@ConfigurationProperties(prefix = "spring.http") 
public class HttpProperties {
    // .....
}
```

去配置文件里面试试前缀，看提示！

![image-20220621142148401](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621142148401.png)

**这就是自动装配的原理！**

​	

> **精髓**

1、SpringBoot启动会加载大量的自动配置类

2、我们看我们需要的功能有没有在SpringBoot默认写好的自动配置类当中；

3、我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件存在在其中，我们就不需要再手动

配置了）

4、给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们只需要在配置文件中指定这些属

性的值即可；

**xxxxAutoConfigurartion：自动配置类；**给容器中添加组件

**xxxxProperties:封装配置文件中相关属性；**

​	

> **了解：@Conditional**

了解完自动装配的原理后，我们来关注一个细节问题，**自动配置类必须在一定的条件下才能生效；**

**@Conditional派生注解（Spring注解版原生的@Conditional作用）**

作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置配里面的所有内容才生效；

![image-20220621142333788](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621142333788.png)

**那么多的自动配置类，必须在一定的条件下才能生效；也就是说，我们加载了这么多的配置类，但不是所有的都**

**生效了。**

我们怎么知道哪些自动配置类生效？

**我们可以通过启用 debug=true属性；来让控制台打印自动配置报告，这样我们就可以很方便的知道哪些自动配置**

**类生效；**

```java
#开启springboot的调试类
debug=true
```

**Positive matches:（自动配置类启用的：正匹配）**

**Negative matches:（没有启动，没有匹配成功的自动配置类：负匹配）**

**Unconditional classes: （没有条件的类）**

---

​	

# 7 SpringBoot Web开发

> **要解决的问题**

- 静态资源的导入，html、css、jsp……
- 首页
- jsp，模板引擎Thymeleaf
- 装配拓展SpringMVC
- 增删改查
- 拦截器
- 国际化

​	

## 7.1 静态资源导入

> **方式一：通过查看SpringBoot底层源码发现，当导入jQuery依赖jar包后，可以通过访问`/webjars`目录下的所有文件；**
>
> **映射地址：localhost:8080/webjars/**

![image-20220621150132181](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621150132181.png)

![image-20220621151155970](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621151155970.png)

​	

> **方式二：从底层源码看出，在classpath下的四个目录也可以导出静态资源，他们的优先级如下：**
>
> **映射地址：localhost:8080/**
>
> **优先级：`resources` > `static`（默认）  > `public`**
>
> <font color="green">**默认的静态资源使用`static`**</font>

![image-20220621150225580](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621150225580.png)

![image-20220621151600511](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621151600511.png)

​	

## 7.2 首页与图标的定制

> **首页**

**启动服务后，访问localhost:8080会默认访问首页index.html**

![image-20220621172712789](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621172712789.png)

​	

<font color="red">**而要注意的是，放在templates目录下的页面，只能通过controller来跳转！相当于在MVC中的WEB-INF目录！！**</font>

![image-20220621173333588](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621173333588.png)

​	

> **图标**

<font color="red">**新版本已取消，如需使用可以降级版本使用！**</font>

在**classpath/public**目录下添加**favicon.ico**文件即可设置图标

![image-20220621174141025](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621174141025.png)

​	

## 7.3 Thymeleaf模板引擎

以前开发是把查出一些数据转发到JSP页面以后，可以通过JSP轻松实现数据的显示以及数据交互等。JSP支持很非

常强大的功能，包括能写Java代码，但是SpringBoot项目首先是以jar的方式，而不是war，其次<font color="red">**SpringBoot使用的是嵌入式Tomcat，所以现在默认不支持JSP。**</font>

**SpringBoot推荐你可以来使用模板引擎：**

模板引擎，我们其实大家听到很多，其实jsp就是一个模板引擎，还有用的比较多的freemarker，包括SpringBoot给

我们推荐的**Thymeleaf**，模板引擎有非常多，但再多的模板引擎，他们的思想都是一样的，什么样一个思想呢我们

来看一下这张图：

![image-20220621174747673](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621174747673.png)

模板引擎的作用就是写一个页面模板，比如有些值呢，是动态的，我们写一些表达式。而这些值，从哪来呢，就是

我们在后台封装一些数据。然后把这个模板和这个数据交给我们模板引擎，模板引擎按照我们这个数据帮你把这表

达式解析、填充到我们指定的位置，然后把这个数据最终生成一个我们想要的内容给我们写出去，这就是我们这个

模板引擎，不管是jsp还是其他模板引擎，都是这个思想。只不过呢，就是说不同模板引擎之间，他们可能这个语

法有点不一样。其他的我就不介绍了，我主要来介绍一下SpringBoot给我们推荐的Thymeleaf模板引擎，这模板引

擎呢，是一个高级语言的模板引擎，他的这个语法更简单。而且呢，功能更强大。

​	

> **引入Thymeleaf**

Thymeleaf 官网：https://www.thymeleaf.org/

Thymeleaf 在Github 的主页：https://github.com/thymeleaf/thymeleaf

Spring官方文档：找到对应的版本

https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter 

找到对应的pom依赖：可以适当点进源码看下本来的包！

```xml
<!--thymeleaf-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

**查看底层源码ThymeleafProperties.class，发现跟SpringMVC的视图解析器类似，由前缀和后缀组合而成。**

![image-20220621180837089](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621180837089.png)

通过controller类跳转，成功！

![image-20220621181151098](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621181151098.png)

​	

<font color="green">**结论：要使用Thymeleaf，只需要导入对应的依赖，其次将html页面放入templates目录下即可！**</font>

​	

## 7.4 Thymeleaf语法

Thymeleaf 官网：https://www.thymeleaf.org/

> **需要查出一些数据，在页面中展示**

1、修改测试请求，增加数据传输；

```java
@Controller
public class IndexController {

    @RequestMapping("/index")
    public String index(Model model) {
        model.addAttribute("msg","Hello,Thymeleaf");
        return "index";
    }
}
```

2、要使用thymeleaf，需要在html文件中导入命名空间的约束，方便提示。

```html
xmlns:th="http://www.thymeleaf.org"
```

3、编写前端页面

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>测试</title>
</head>
<body>
<h1>测试页面</h1>

<div th:text="${msg}"></div>
</body>
</html>
```

![image-20220621220753675](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621220753675.png)

​	

**1、我们可以使用任意的 th:attr 来替换Html中原生属性的值！**

![image-20220621183439176](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621183439176.png)

**2、能写哪些表达式呢？**

```html
Simple expressions:（表达式语法）
Variable Expressions: ${...}：获取变量值；OGNL；
    1）、获取对象的属性、调用方法
    2）、使用内置的基本对象：#18
         #ctx : the context object.
         #vars: the context variables.
         #locale : the context locale.
         #request : (only in Web Contexts) the HttpServletRequest object.
         #response : (only in Web Contexts) the HttpServletResponse object.
         #session : (only in Web Contexts) the HttpSession object.
         #servletContext : (only in Web Contexts) the ServletContext object.

    3）、内置的一些工具对象：
　　　　　　#execInfo : information about the template being processed.
　　　　　　#uris : methods for escaping parts of URLs/URIs
　　　　　　#conversions : methods for executing the configured conversion service (if any).
　　　　　　#dates : methods for java.util.Date objects: formatting, component extraction, etc.
　　　　　　#calendars : analogous to #dates , but for java.util.Calendar objects.
　　　　　　#numbers : methods for formatting numeric objects.
　　　　　　#strings : methods for String objects: contains, startsWith, prepending/appending, etc.
　　　　　　#objects : methods for objects in general.
　　　　　　#bools : methods for boolean evaluation.
　　　　　　#arrays : methods for arrays.
　　　　　　#lists : methods for lists.
　　　　　　#sets : methods for sets.
　　　　　　#maps : methods for maps.
　　　　　　#aggregates : methods for creating aggregates on arrays or collections.
==================================================================================

  Selection Variable Expressions: *{...}：选择表达式：和${}在功能上是一样；
  Message Expressions: #{...}：获取国际化内容
  Link URL Expressions: @{...}：定义URL；
  Fragment Expressions: ~{...}：片段引用表达式

Literals（字面量）
      Text literals: 'one text' , 'Another one!' ,…
      Number literals: 0 , 34 , 3.0 , 12.3 ,…
      Boolean literals: true , false
      Null literal: null
      Literal tokens: one , sometext , main ,…
      
Text operations:（文本操作）
    String concatenation: +
    Literal substitutions: |The name is ${name}|
    
Arithmetic operations:（数学运算）
    Binary operators: + , - , * , / , %
    Minus sign (unary operator): -
    
Boolean operations:（布尔运算）
    Binary operators: and , or
    Boolean negation (unary operator): ! , not
    
Comparisons and equality:（比较运算）
    Comparators: > , < , >= , <= ( gt , lt , ge , le )
    Equality operators: == , != ( eq , ne )
    
Conditional operators:条件运算（三元运算符）
    If-then: (if) ? (then)
    If-then-else: (if) ? (then) : (else)
    Default: (value) ?: (defaultvalue)
    
Special tokens:
    No-Operation: _
```

​	

**测试一下**

1、 编写一个Controller，放一些数据

```java

@RequestMapping("/t2")
public String test2(Map<String,Object> map){
    //存入数据
    map.put("msg","<h1>Hello</h1>");
    map.put("users", Arrays.asList("xleixz","xleixz"));
    //classpath:/templates/test.html
    return "test";
}
```

2、测试页面取出数据

```html

<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>测试</title>
</head>
<body>
<h1>测试页面</h1>

<div th:text="${msg}"></div>
<!--不转义-->
<div th:utext="${msg}"></div>

<!--遍历数据-->
<!--th:each每次遍历都会生成当前这个标签：官网#9-->
<h4 th:each="user :${users}" th:text="${user}"></h4>

<h4>
    <!--行内写法：官网#12-->
    <span th:each="user:${users}">[[${user}]]</span>
</h4>

</body>
</html>
```

3、启动测试即可

​	

## 7.5 接管SpringMVC，实现MVC自动配置

全面接管即：SpringBoot对SpringMVC的自动配置不需要了，所有都是我们自己去配置！只需在我们的配置类中要

加一个`@EnableWebMvc`。

我们看下如果我们全面接管了SpringMVC了，我们之前SpringBoot给我们配置的静态资源映射一定会无效。

另外当项目中涉及大量的页面跳转，我们可以使用`addViewControllers`方法实现无业务逻辑跳转，从而减少控

制器代码的编写。

<font color="green">**addViewControllers方法可以实现将一个请求直接映射为视图，不需要编写控制器来实现，从而简化了页面跳转。**</font>

​	

## 7.6 报错问题

> **Thymeleaf 表达式报红波浪线**

修改IDEA设置：把设置里面的 Editor->Inspections->Thymeleaf->Expression variables validation后面的勾去掉

![image-20220621221212584](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220621221212584.png)

---

​	

# 8 员工管理系统

## 8.1 准备工作

1. 准备数据库

   ```mysql
   Create DATABASE company;
   
   use company;
   
   create table employee(id INTEGER primary key auto_increment,
   											lastName varchar(255) not null,
   											email varchar(255) not null,
   											gender INTEGER not null,
   											birth varchar(255) not null
   											);
   											
   INSERT into employee(lastName,email,gender,birth)
   								values("张三","zhangsan@163.com",1,"2000-01-08"),
   											("李四","lisi@163.com",1,"1998-01-19"),
   											("张华","zhanghua@163.com",1,"2000-10-25"),
   											("吴磊","wulei@163.com",1,"2001-02-21"),
   											("高强","gaoqiang@163.com",1,"1999-11-02");
   											
   
   ```

   ```mysql
   use company;
   
   create table department(id INTEGER primary key auto_increment,
   											departmentName varchar(255) not null
   											);
   											
   INSERT into department(departmentName)
   								values("陈露"),
   											("张颖"),
   											("吴国华"),
   											("三毛"),
   											("朱翠莲");
   
   ```

2. 实体类

   ```java
   public class Department {
   
       private Integer id;
       private String departmentName;
   
    //getter、setter、有参、无参、toString
   }
   ```

   ```java
   public class Employee {
   
       private Integer id;
       private String lastName;
       private String email;
       private Integer gender;//0: 女  1: 男
   
       private Department department;
       private String birth;
   
       public Employee() {
       }
   		//getter、setter、有参、无参、toString
   }
   ```

3. 重写视图解析器，不需要controller【/config/MyMvcConfig】

   ```java
   //全面拓展SpringMVC
   @Configuration
   public class MyMvcConfig implements WebMvcConfigurer {
   
       @Override
       public void addViewControllers(ViewControllerRegistry registry) {
           registry.addViewController("/").setViewName("index");
           registry.addViewController("/index.html").setViewName("index");
       }
   }
   ```

​	

## 8.2 页面配置

**页面配置：注意所有的静态资源都需要使用Thymeleaf接管，链接用`@{/}"`**

首页【index.html】

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
		<meta name="description" content="">
		<meta name="author" content="">
		<title>Signin Template for Bootstrap</title>
		<!-- Bootstrap core CSS -->
		<link th:href="@{/css/bootstrap.min.css}" rel="stylesheet">
		<!-- Custom styles for this template -->
		<link th:href="@{/css/signin.css}" rel="stylesheet">
	</head>

	<body class="text-center">
		<form class="form-signin" th:action="@{/user/login}" method="post">
			<img class="mb-4" th:src="@{/img/bootstrap-solid.svg}" alt="" width="72" height="72">
			<h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}">Please sign in</h1>
			<p style="color: red" th:text="${msg}"></p>
			<input type="text" class="form-control" name="username" th:placeholder="#{login.username}" required="" autofocus="">
			<input type="password" class="form-control" name="password" th:placeholder="#{login.password}" required="">
			<div class="checkbox mb-3">
				<label>
          <input type="checkbox" value="remember-me" > [[#{login.remember}]]
        </label>
			</div>
			<button class="btn btn-lg btn-primary btn-block" type="submit">[[#{login.btn}]]</button>
			<p class="mt-5 mb-3 text-muted">© 2017-2018</p>
			<a class="btn btn-sm" th:href="@{/index.html(l='zh_cn')}">中文</a>
			<a class="btn btn-sm" th:href="@{/index.html(l='en_US')}">English</a>
		</form>

	</body>

</html>
```

​	

## 8.3 页面国际化

所谓的页面国际化就是在网页上实现中英文切换。

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220622104008983.png" alt="image-20220622104008983" style="zoom: 50%;" />

​	

1. 确保IDEA的编码全是UTF-8

   ![image-20220622104250374](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220622104250374.png)

2. 在**resources目录下新建一个目录【i18n】（国际化）**，在**【i18n】目录下创建login.properties和login_zh_CN.properties配置文件，可自动合并为一个目录**

   ![image-20220622105154808](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220622105154808.png)

3. 使用可视化编辑配置文件，配置文件会自动生成

   ![image-20220622105313175](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220622105313175.png)

   ![image-20220622105414155](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220622105414155.png)

4. 到【application.properties】文件中配置

   ```properties
   # 配置文件的真实位置
   spring.messages.basename=i18n.login
   ```

   ![image-20220622105802189](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220622105802189.png)

5. 修改每一个位置

   ```html
   <h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}">Please sign in</h1>
   ```

6. 看一下效果

   ![image-20220622110815256](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220622110815256.png)

   ​	

> **配置根据按钮自动切换中文英文！**

**配置国际化解析**

在Spring中有一个国际化的Locale （区域信息对象）；里面有一个叫做LocaleResolver （获取区域信息对象）的解

析器！SpringBoot默认配置：

```java
@Bean
@ConditionalOnMissingBean
@ConditionalOnProperty(prefix = "spring.mvc", name = "locale")
public LocaleResolver localeResolver() {
       // 容器中没有就自己配，有的话就用用户配置的
       if (this.mvcProperties.getLocaleResolver() == WebMvcProperties.LocaleResolver.FIXED) {
           return new FixedLocaleResolver(this.mvcProperties.getLocale());
       }
       // 接收头国际化分解
       AcceptHeaderLocaleResolver localeResolver = new AcceptHeaderLocaleResolver();
       localeResolver.setDefaultLocale(this.mvcProperties.getLocale());
       return localeResolver;
}
```

AcceptHeaderLocaleResolver 这个类中有一个方法

   ```java
   public Locale resolveLocale(HttpServletRequest request) {
       Locale defaultLocale = this.getDefaultLocale();
       // 默认的就是根据请求头带来的区域信息获取Locale进行国际化
       if (defaultLocale != null && request.getHeader("Accept-Language") == null) {
           return defaultLocale;
       } else {
           Locale requestLocale = request.getLocale();
           List<Locale> supportedLocales = this.getSupportedLocales();
           if (!supportedLocales.isEmpty() && !supportedLocales.contains(requestLocale)) {
               Locale supportedLocale = this.findSupportedLocale(request, supportedLocales);
               if (supportedLocale != null) {
                   return supportedLocale;
               } else {
                   return defaultLocale != null ? defaultLocale : requestLocale;
               }
           } else {
               return requestLocale;
           }
       }
   }
   ```

点击链接让我们的国际化资源生效，就需要让我们自己的Locale生效！写一个自己的LocaleResolver，可以在链接

上携带区域信息！

修改一下前端页面的跳转连接：

   ```html
   <!-- 这里传入参数不需要使用 ？使用 （key=value）-->
   <a class="btn btn-sm" th:href="@{/index.html(l='zh_CN')}">中文</a>
   <a class="btn btn-sm" th:href="@{/index.html(l='en_US')}">English</a>
   ```

写一个处理的组件类！

   ```java
   //可以在链接上携带区域信息
   public class MyLocaleResolver implements LocaleResolver {
   
       //解析请求
       @Override
       public Locale resolveLocale(HttpServletRequest request) {
   
           String language = request.getParameter("l");
           Locale locale = Locale.getDefault(); // 如果没有获取到就使用系统默认的
           //如果请求链接不为空
           if (!StringUtils.isEmpty(language)){
               //分割请求参数
               String[] split = language.split("_");
               //国家，地区
               locale = new Locale(split[0],split[1]);
           }
           return locale;
       }
   
       @Override
       public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {
   
       }
   }
   ```

为了让区域化信息能够生效，需要再配置一下这个组件！在MvcConofig下添加bean；

```java
   @Bean
   public LocaleResolver localeResolver(){
       return new MyLocaleResolver();
   }
```

启动测试：

![测试国际化](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/测试国际化.gif)

   ​	

## 8.4 登录功能实现

1、首先将`action`请求不写死

```html
<form class="form-signin" th:action="@{/user/login}" method="post">
```

2、在controller控制类里写请求方法

```java
@Controller
public class LoginController {

    @RequestMapping("/user/login")
    public String login(@RequestParam("username") String username, @RequestParam("password") String password, Model model) {
        //如果用户名不为空,且密码为123456
        if (!StringUtils.isEmpty(username)&&"123456".equals(password)){
            //登录成功
            return "dashboard.html";
        }else {
            //登录失败
            model.addAttribute("msg","用户名或者密码错误!");
            return "index";
        }
    }
}
```

3、在前端页面写用户名或密码错误提示**（使用Thymeleaf语法）**

```html
 <!--如果msg的值为空，则不显示提示-->
    <p style="color: red" th:text="${msg}" th:if="${not #strings.isEmpty(msg)}"></p>
```

![image-20220622162812263](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220622162812263.png)4、测试发现，地址栏显示用户名和密码，不安全

<font color="green">**方式一：通过修改请求方式，改为post请求即可！**</font>

```java
@PostMapping("/user/login")
```

<font color="green">**方式二：通过addViewControllers方法实现将请求直接映射为视图！**</font>

1、在**config**目录下的【MyMvcConfig.java】添加

```java
//全面拓展SpringMVC
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
	@Override
    public void addViewControllers(ViewControllerRegistry registry) {
           registry.addViewController("/main.html").setViewName("dashboard");
    }
}
```

2、修改controller类中为重定向请求

```java
return "redirect:/main.html";
```

```java
@Controller
public class LoginController {

    @RequestMapping("/user/login")
    public String login(@RequestParam("username") String username, @RequestParam("password") String password, Model model) {
        //如果用户名不为空,且密码为123456
        if (!StringUtils.isEmpty(username)&&"123456".equals(password)){
            //登录成功
            return "redirect:/main.html";
        }else {
            //登录失败
            model.addAttribute("msg","用户名或者密码错误!");
            return "index";
        }
    }
}
```

![image-20220622163453594](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220622163453594.png)   

  			

## 8.5 登录拦截器

<font color="red">**问题：不登录用户名和密码也可以登录！**</font>

<font color="green">**解决：添加拦截器**</font>

1、在**config**目录下新建一个类【LoginHandlerInterceptor.java】**实现HandlerInterceptor**类，就是一个拦截器

```java
public class LoginHandlerInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        //登录成功之后,应该有用户的session
        Object loginUser = request.getSession().getAttribute("loginUser");

        //没有登录
        if (loginUser==null){
            request.setAttribute("msg","没有权限，请先登录!");
            request.getRequestDispatcher("/index.html").forward(request,response);
            return false;
        }else{
            return true;
        }

    }

}
```

2、在controller类里绑定**session**，将`username`绑定到session中，为了给拦截器判断

```java
session.setAttribute("loginUser",username);
```

```java
@Controller
public class LoginController {

    @RequestMapping("/user/login")
    public String login(@RequestParam("username") String username,
                        @RequestParam("password") String password,
                        Model model,
                        HttpSession session) {
        //如果用户名不为空,且密码为123456
        if (!StringUtils.isEmpty(username) && "123456".equals(password)) {
            //登录成功
            session.setAttribute("loginUser",username);
            return "redirect:/main.html";
        } else {
            //登录失败
            model.addAttribute("msg", "用户名或者密码错误!");
            return "index";
        }
    }
}
```

3、在**config**目录下的【MyMvcConfig.java】重写拦截器方法【addInterceptors】

```java
//拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginHandlerInterceptor())
                .addPathPatterns("/**")
                .excludePathPatterns("/index.html", "/", "/user/login","/css/*","/js/**","/img/**");
}
```

4、启动测试，成功！

​	

## 8.6 展示员工列表

> **用Thymeleaf语法抽取侧边栏**

当多个页面跳转时，顶部与侧边栏不需要改变时，可以通过组件**将需要复用的部分**抽取出来，单独放到一个页面，使用Thymeleaf语法抽取`th:fragment=""`

![image-20220622171926829](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220622171926829.png)



 然后在需要使用的页面插入即可`th:insert="~{::}"`

![image-20220622172132785](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220622172132785.png)

​	

> **使用Thymeleaf模板引擎遍历数据**

**语法：**

```html
<tr th:each="emp:${emps}">
	<td th:text="${emp.getId()}"></td>
</tr>
```

`:`前的**emp**是为取的名字，`:`后的**${emps}**是从后端`model.addAttribute("msg","数据")`传来的

![image-20220622173045124](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220622173045124.png)

​	

> **Thymeleaf的三元判断**

pojo中定义的性别表示方式为0为女，1为男，则可以使用**Thymeleaf**的三元表达式来优化显示

```html
<td th:text="${emp.getGender()==0?'女':'男'}"></td>
```

```html

```

![image-20220622173439427](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220622173439427.png)

​	

> **Thymeleaf时间的表示**

```html
<td th:text="${#dates.format(emp.getBirth(),'yyyy-MM-dd HH:mm:ss')}"></td>
```

![image-20220622174413812](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220622174413812.png)

​	

## 8.7 添加员工实现

首先准备添加员工信息的页面，其次编写controller类实现跳转，将遍历出的信息绑定到model，通过转发到页面显示。

![image-20220622202844563](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220622202844563.png)

```html
<option th:each="dept:${departments}" th:text="${dept.getDepartmantName()}" th:value="${dept.getId()}"></option>
```

`th:each`：**遍历departments集合，并且命名为dept**

`th:text`：**option的值，pojo实体类中的get方法，text只负责显示，给用户选择**

`th:value`：**value负责赋值，传递到后端**

![image-20220622203114051](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220622203114051.png)

​	

> **时间日期格式化**

因为默认的日期格式是yy/MM/dd，如果前端输入的格式如果为2019-01-01，则可以通过在**application.properties**中

配置时间日期格式化。

```properties
spring.mvc.date-format=yyyy-MM-dd
```

![image-20220622204323722](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220622204323722.png)

​	

## 8.8 404 和 500页面

SpringBoot提供了404和505页面，只需要把html命名为404或500，即可作为默认的400和500页面。

​	

## 8.9 注销

在controller类中，设置移除`session`即可。

```java
session.invalidate();
```

![image-20220622212458334](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220622212458334.png)

​	

## 8.10 开发总结

后台模板：

- **X-admin**：[X-admin - 经典前端网站后台管理模板框架 (xuebingsi.com)](http://x.xuebingsi.com/)
- **Layui**：[Layui - 经典开源模块化前端 UI 框架 (itze.cn)](https://layui.itze.cn/)
- **Bootstrap**：[Bootstrap网站模板_Bootstrap网站模板免费下载_模板之家 (cssmoban.com)](http://www.cssmoban.com/tags/Qm9vdHN0cmFw/)
- **Semantic UI**：[Semantic UI (semantic-ui.com)网站模板，图标logo](https://semantic-ui.com/)

---

​	

# 9 Spring Data

## 9.1 Spring Data简介

对于数据访问层，无论是 SQL(关系型数据库) 还是 NOSQL(非关系型数据库)，Spring Boot 底层都是采用 Spring

Data 的方式进行统一处理。

Spring Boot 底层都是采用 Spring Data 的方式进行统一处理各种数据库，Spring Data 也是 Spring 中与 Spring 

Boot、Spring Cloud 等齐名的知名项目。

Sping Data 官网：https://spring.io/projects/spring-data

数据库相关的启动器 ：可以参考官方文档：

https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter

​	

## 9.2 Spring Boot整合JDBC

### 9.2.1 SPringBoot默认数据源

1、新建Spring Boot项目，引入相应的模块！

![image-20220623164156964](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220623164156964.png)

2、查看pom时会发现已自动导入驱动jar包

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<dependency>
	<groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
	<scope>runtime</scope>
</dependency>
```

3、编写yml配置文件连接数据库（mysql 8.0+ 要配时区）

```yaml
spring:
  datasource:
    username: root
    password: 123456
    url: jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characterEncoding=utf-8&serverTimezone=UTC
    driver-class-name: com.mysql.cj.jdbc.Driver
```

4、配置完后，直接就可以在SpringBoot提供的测试类进行测试了

```java
@SpringBootTest
class SpringBoot04DataApplicationTests {

    @Autowired
    DataSource dataSource;

    @Test
    void contextLoads() throws SQLException {

        //查看默认数据源  class com.zaxxer.hikari.HikariDataSource
        System.out.println(dataSource.getClass());

        //获得数据库连接
        Connection connection = dataSource.getConnection();
        System.out.println(connection);
        //关闭连接
        connection.close();
    }
}
```

测试发现**它默认给的配置的数据源为 : class com.zaxxer.hikari.HikariDataSource ， 没有手动配置**

![image-20220623170942314](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220623170942314.png)

根据全局搜索，找到数据源的所有自动配置都在 ：DataSourceAutoConfiguration文件

```java
@Import(
    {Hikari.class, Tomcat.class, Dbcp2.class, Generic.class, DataSourceJmxConfiguration.class}
)
protected static class PooledDataSourceConfiguration {
    protected PooledDataSourceConfiguration() {
    }
}
```

可以看出 Spring Boot 2.2.5 默认使用**HikariDataSource 数据源**，而以前版本，如 Spring Boot 1.5 默认使用

`org.apache.tomcat.jdbc.poll.DataSource`作为数据源。

<font color="red">**HikariDataSource 号称 Java WEB 当前速度最快的数据源，相比于传统的 C3P0 、DBCP、Tomcat jdbc 等连接池更加优秀。**</font>

**可以使用 spring.datasource.type 指定自定义的数据源类型，值为 要使用的连接池实现的完全限定名。**

**到这里就可以做CRUD操作了。**

​	

### 9.2.2 JdbcTemplate模板

如果遇到`xxxx Template`，那些就是SpringBoot已经配置好的模板bean，开箱即用。

阅读底层源码总结：

**JdbcTemplate主要提供以下几类方法：**

- execute方法：可以用于执行任何SQL语句，一般用于执行DDL语句；

- update方法及batchUpdate方法：update方法用于执行新增、修改、删除等语句；batchUpdate方法用于执行批

  处理相关语句；

- query方法及queryForXXX方法：用于执行查询相关语句；

- call方法：用于执行存储过程、函数相关语句。

**测试一下：**

```java
@RestController
public class JDBCController {

    /**
     * Spring Boot 默认提供了数据源，默认提供了 org.springframework.jdbc.core.JdbcTemplate
     * JdbcTemplate 中会自己注入数据源，用于简化 JDBC操作
     * 还能避免一些常见的错误,使用起来也不用再自己来关闭数据库连接
     */
    @Autowired
    JdbcTemplate jdbcTemplate;

    //查询employee表中所有数据
    //没有实体类，数据库中的东西可以用Map获取
    //List 中的1个 Map 对应数据库的 1行数据
    //Map 中的 key 对应数据库的字段名，value 对应数据库的字段值
    @GetMapping("/list")
    public List<Map<String, Object>> userList() {
        String sql = "select * from user";
        List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
        return maps;
    }

    //新增一个用户
    @GetMapping("/add")
    public String addUser() {
        //插入语句，注意时间问题
        String sql = "insert into user(id, name,pwd)values (10,'小黄','123456')";
        jdbcTemplate.update(sql);
        //查询
        return "addOk";
    }

    //修改用户信息
    @GetMapping("/update/{id}")
    public String updateUser(@PathVariable("id") int id) {
        //插入语句
        String sql = "update user set name=?,pwd=? where id=" + id;
        //数据
        Object[] objects = new Object[2];
        objects[0] = "测试员2";
        objects[1] = "1233321";
        jdbcTemplate.update(sql, objects);
        //查询
        return "updateOk";
    }

    //删除用户
    //不使用RestFul风格传id
    @GetMapping("/delete")
    public String delUser(int id) {
        //插入语句
        String sql = "delete from user where id=?";
        jdbcTemplate.update(sql, id);
        //查询
        return "deleteOk";
    }
}
```

**到此，CURD的基本操作，使用 JDBC 就搞定了。**

​	

## 9.3 Spring Boot集成Druid数据源

### 9.3.1 Druid简介

**Druid**是阿里巴巴开源平台上一个数据库连接池实现，结合了C3P0、DBCP、PROXOOL等DB池的优点，同时加入了日志监控。

Druid开源很好的监控DB池连接和SQL执行情况，天生就是针对监控而生的DB连接池。

**SpringBoot 2.0 以上默认使用Hikari（黑卡瑞）数据源，Hikari与Druid（德鲁伊，发音：纠瑞德）都是当前JavaWeb上最优秀的数据源。**

Github地址：https://github.com/alibaba/druid/

![image-20220623191638944](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220623191638944.png)

![image-20220623191741725](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220623191741725.png)

![image-20220623191752323](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220623191752323.png)

​	

### 9.3.2 配置数据源

1、添加上 Druid 数据源依赖；

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.8</version>
</dependency>
```

2、切换数据源；**Spring Boot 2.0 以上默认使用 com.zaxxer.hikari.HikariDataSource 数据源**，但可以通过 `spring.datasource.type` 指定数据源；

```yaml
spring:
  datasource:
    username: root
    password: 123456
    url: jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characterEncoding=utf-8&serverTimezone=UTC
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
```

3、数据源切换之后，在测试类中注入 DataSource，然后获取到它，测试输出看一下；

![image-20220623192632259](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220623192632259.png)

4、切换成功就可以**设置数据源连接初始化大小、最大连接数、等待时间、最小连接数 等设置项；**

> 配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
> 如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
> 则导入 log4j 依赖即可，Maven 地址：https://mvnrepository.com/artifact/log4j/log4j

```yaml
spring:
  datasource:
    username: root
    password: 123456
    url: jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characterEncoding=utf-8&serverTimezone=UTC
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource

    #Spring Boot 默认是不注入这些属性值的，需要自己绑定
    #druid 数据源专有配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true

    #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
    #如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
    #则导入 log4j 依赖即可，Maven 地址：https://mvnrepository.com/artifact/log4j/log4j
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
```

5、导入Log4j 的依赖

```xml
<!-- https://mvnrepository.com/artifact/log4j/log4j -->
<dependency>
	<groupId>log4j</groupId>
	<artifactId>log4j</artifactId>
	<version>1.2.12</version>
</dependency>
```

6、现在需要程序员自己为 DruidDataSource 绑定全局配置文件中的参数，再添加到容器中，而不再使用 Spring Boot 的自动生成了；我们需要 自己添加 DruidDataSource 组件到容器中，并绑定属性；**这个config对应的是原来的bean.xml，将application.yml配置文件与config配置绑定**

```java
@Configuration
public class DruidConfig {

    /*
       将自定义的 Druid数据源添加到容器中，不再让 Spring Boot 自动创建
       绑定全局配置文件中的 druid 数据源属性到 com.alibaba.druid.pool.DruidDataSource从而让它们生效
       @ConfigurationProperties(prefix = "spring.datasource")：作用就是将 全局配置文件中
       前缀为 spring.datasource的属性值注入到 com.alibaba.druid.pool.DruidDataSource 的同名参数中
     */
    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druidDataSource() {
        return new DruidDataSource();
    }
}
```

7、测试类中测试一下；

```java
@SpringBootTest
class SpringBoot04DataApplicationTests {

    @Autowired
    DataSource dataSource;

    @Test
    void contextLoads() throws SQLException {

        //查看默认数据源  class com.zaxxer.hikari.HikariDataSource
        System.out.println(dataSource.getClass());

        //获得数据库连接
        Connection connection = dataSource.getConnection();
        System.out.println(connection);

        DruidDataSource druidDataSource = (DruidDataSource) dataSource;
        System.out.println("druidDataSource 数据源最大连接数：" + druidDataSource.getMaxActive());
        System.out.println("druidDataSource 数据源初始化连接数：" + druidDataSource.getInitialSize());
        //关闭连接
        connection.close();
    }
}
```

输出结果 ：可见配置参数已经生效！

![image-20220623200549744](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220623200549744.png)

​	

### 9.3.3 配置Druid数据源监控

Druid 数据源具有监控的功能，并提供了一个 web 界面方便用户查看，类似安装 路由器时，提供了一个默认的 web 页面。

所以第一步需要设置 Druid 的后台管理页面，比如登录账号、密码 等；配置后台管理；

```java
@Configuration
public class DruidConfig {

    /*
       将自定义的 Druid数据源添加到容器中，不再让 Spring Boot 自动创建
       绑定全局配置文件中的 druid 数据源属性到 com.alibaba.druid.pool.DruidDataSource从而让它们生效
       @ConfigurationProperties(prefix = "spring.datasource")：作用就是将 全局配置文件中
       前缀为 spring.datasource的属性值注入到 com.alibaba.druid.pool.DruidDataSource 的同名参数中
     */
    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druidDataSource() {
        return new DruidDataSource();
    }

    //配置 Druid 监控管理后台的Servlet；
	//内置 Servlet 容器时没有web.xml文件，所以使用 Spring Boot 的注册 Servlet 方式
    @Bean
    public ServletRegistrationBean statViewServlet() {
        ServletRegistrationBean bean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");

        // 这些参数可以在 com.alibaba.druid.support.http.StatViewServlet
        // 的父类 com.alibaba.druid.support.http.ResourceServlet 中找到
        Map<String, String> initParams = new HashMap<>();
        initParams.put("loginUsername", "admin"); //后台管理界面的登录账号
        initParams.put("loginPassword", "123456"); //后台管理界面的登录密码

        //后台允许谁可以访问
        //initParams.put("allow", "localhost")：表示只有本机可以访问
        //initParams.put("allow", "")：为空或者为null时，表示允许所有访问
        initParams.put("allow", "");
        //deny：Druid 后台拒绝谁访问
        //initParams.put("xleixz", "192.168.1.20");表示禁止此ip访问

        //设置初始化参数
        bean.setInitParameters(initParams);
        return bean;
    }
}
```

配置完毕后，我们可以选择访问 ：http://localhost:8080/druid

![image-20220623200823603](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220623200823603.png)

输入设置的用户名和密码`admin  123456`进入

![image-20220623200847892](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220623200847892.png)

它能监控SQL的任何操作；

![image-20220623201611404](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220623201611404.png)

​	

### 9.3.4 配置 Druid web 监控 filter 过滤器（了解）

```java
//配置 Druid 监控 之  web 监控的 filter
//WebStatFilter：用于配置Web和Druid数据源之间的管理关联监控统计
@Bean
public FilterRegistrationBean webStatFilter() {
    FilterRegistrationBean bean = new FilterRegistrationBean();
    bean.setFilter(new WebStatFilter());

    //exclusions：设置哪些请求进行过滤排除掉，从而不进行统计
    Map<String, String> initParams = new HashMap<>();
    initParams.put("exclusions", "*.js,*.css,/druid/*,/jdbc/*");
    bean.setInitParameters(initParams);

    //"/*" 表示过滤所有请求
    bean.setUrlPatterns(Arrays.asList("/*"));
    return bean;
}
```

在工作中，按需求进行配置即可，主要用作监控！

---

​		

## 9.4 Spring Boot整合Mybatis

官方文档：http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/

Maven仓库地址：https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter/2.1.1

![image-20220623174658711](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220623174658711.png)

![image-20220623202442983](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220623202442983.png)

​	

<table><tr><td bgcolor=PowderBlue><b>整合测试<b></td></tr></table>

**1、导入 MyBatis 所需要的依赖**

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.3</version>
</dependency>
```

**2、配置数据库连接信息【application.yml】**

```yaml
spring:
  datasource:
    username: root
    password: 123456
    url: jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characterEncoding=utf-8&serverTimezone=UTC
    driver-class-name: com.mysql.cj.jdbc.Driver

```

**3、测试数据库是否连接成功！**

![image-20220623235126960](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220623235126960.png)

**4、创建pojo实体类【User.java】**

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {

    private int id;
    private String name;
    private String pwd;
}
```

**5、创建mapper目录以及对应的 Mapper 接口【UserMapper.java】**

```java
//@Mapper注解: 表示这是一个Mybatis的Mapper类, 也可以在Application类中使用@MapperScan("")扫描的方式
@Mapper
//dao层，可以使用万能的@Component，也可以使用分层的@Repository
//等价于 <bean id="UserMapper" class="com.xleixz.mapper.UserMapper"/>
@Repository
public interface UserMapper {

    List<User> queryUserList();

    User queryUserById(int id);

    int addUser(User user);

    int updateUser(User user);

    int deleteUser(int id);
}
```

**6、在*<font color="red">application.yml</font>*配置文件中，整合Mybatis，起别名，绑定Mapper.xml映射文件**

```yaml
# 整合Mybatis
mybatis:
  type-aliases-package: com.xleixz.pojo
  mapper-locations: classpath:mybatis/mapper/*.xml
```

**7、<font color="red">在*resources目录*下创建</font>对应的Mapper映射文件**

![image-20220624001254508](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220624001254508.png)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.xleixz.mapper.UserMapper">

    <!--开启缓存-->
    <cache/>

    <select id="queryUserList" resultType="user">
        select *
        from user;
    </select>

    <select id="queryUserById" resultType="user" parameterType="int">
        select *
        from user
        where id = #{id};
    </select>

</mapper>
```

**9、service业务层，业务实现接口和业务实现类，<font color=red>时刻记住：service业务层调dao/mapper层，controller控制层调service业务层</font>**

```java
//@Mapper注解: 表示这是一个Mybatis的Mapper类, 也可以在Application类中使用@MapperScan("")扫描的方式
@Mapper
//dao层，可以使用万能的@Component，也可以使用分层的@Repository
//等价于 <bean id="UserMapper" class="com.xleixz.mapper.UserMapper"/>
@Service
public interface UserService {

    List<User> queryUserList();

    User queryUserById(int id);

    int addUser(User user);

    int updateUser(User user);

    int deleteUser(int id);
}
```

```java
@Service
public class UserServiceImpl implements UserService {

    @Autowired
    private UserMapper userMapper;

    @Override
    public List<User> queryUserList() {
        return userMapper.queryUserList();
    }

    @Override
    public User queryUserById(int id) {
        return userMapper.queryUserById(id);
    }

    @Override
    public int addUser(User user) {
        return userMapper.addUser(user);
    }

    @Override
    public int updateUser(User user) {
        return userMapper.updateUser(user);
    }

    @Override
    public int deleteUser(int id) {
        return userMapper.deleteUser(id);
    }
}
```

**10、maven配置资源过滤问题**

```xml
<resources>
    <resource>
        <directory>src/main/java</directory>
        <includes>
            <include>**/*.xml</include>
        </includes>
        <filtering>true</filtering>
    </resource>
</resources>
```

<table><tr><td bgcolor=PowderBlue><b>整合完毕</b></td></tr></table>

---

​	

# 11 SpringSecurity

再Web开发中，安全是第一位！虽然过滤器，拦截器等都可以实现，但是利用框架使用更方便，简单。

**安全属于非功能性需求；**

如果在应用开发的后期才考虑安全的问题，就可能陷入一个两难的境地：一方面，应用存在严重的安全漏洞，无法满足用户的要求，并可能造成用户的隐私数据被攻击者窃取；另一方面，应用的基本架构已经确定，要修复安全漏洞，可能需要对系统的架构做出比较重大的调整，因而需要更多的开发时间，影响应用的发布进程。

<font color=35B7FD>**建议在开发初期就把安全的因素考虑进来。**</font>

<font color=35B7FD>**市面上知名的安全框架：shiro、SpringSecurity；**</font>他们除了类不一样，名字不一样，其他都类似；

<font color=ff0000>主要功能：认证和授权</font>，例如VIP1用户和VIP2用户，为了限制登录，原来是设置拦截器，现在只需要设置授权即可。
Spring 是一个非常流行和成功的 Java 应用开发框架。Spring Security 基于 Spring 框架，提供了一套 Web 应用安全性的完整解决方案。一般来说，Web 应用的安全性包括用户认证（Authentication）和用户授权（Authorization）两个部分。用户认证指的是验证某个用户是否为系统中的合法主体，也就是说用户能否访问该系统。用户认证一般要求用户提供用户名和密码。系统通过校验用户名和密码来完成认证过程。用户授权指的是验证某个用户是否有权限执行某个操作。在一个系统中，不同用户所具有的权限是不同的。比如对一个文件来说，有的用户只能进行读取，而有的用户可以进行修改。一般来说，系统会为不同的用户分配不同的角色，而每个角色则对应一系列的权限。

​	

**Spring Security 是针对Spring项目的安全框架，也是Spring Boot底层安全模块默认的技术选型，他可以实现强大的Web安全控制，对于安全控制，仅需要引入 spring-boot-starter-security 模块，进行少量的配置，即可实现强大的安全管理！**

<font color=green>**记住几个类：**</font>

- WebSecurityConfigurerAdapter：自定义Security策略
- AuthenticationManagerBuilder：自定义认证策略
- @EnableWebSecurity：开启WebSecurity模式

**Spring Security的两个主要目标是 “认证” 和 “授权”（访问控制）。**

**“认证”（Authentication）**

身份验证是关于验证您的凭据，如用户名/用户ID和密码，以验证您的身份。

身份验证通常通过用户名和密码完成，有时与身份验证因素结合使用。

 **“授权” （Authorization）**

授权发生在系统成功验证您的身份后，最终会授予您访问资源（如信息，文件，数据库，资金，位置，几乎任何内容）的完全权限。

这个概念是通用的，而不是只在Spring Security 中存在。

​	

## 11.1 认证和授权

目前，我们的测试环境，是谁都可以访问的，我们使用 Spring Security 增加上认证和授权的功能

**1、引入 Spring Security 模块**

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

**2、编写 Spring Security 配置类**

参考官网：https://spring.io/projects/spring-security 

查看项目中的版本，找到对应的帮助文档：

https://docs.spring.io/spring-security/site/docs/5.3.0.RELEASE/reference/html5  #servlet-applications 8.16.4

![image-20220624102607471](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220624102607471.png)

**3、编写基础配置类**

```java
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@EnableWebSecurity // 开启WebSecurity模式
public class SecurityConfig extends WebSecurityConfigurerAdapter {

   @Override
   protected void configure(HttpSecurity http) throws Exception {
       
  }
}
```

**4、在编写的基础类中定制请求的授权规则**

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
   // 定制请求的授权规则
   // 首页所有人可以访问
   http.authorizeRequests().antMatchers("/").permitAll()
  .antMatchers("/level1/**").hasRole("vip1")
  .antMatchers("/level2/**").hasRole("vip2")
  .antMatchers("/level3/**").hasRole("vip3");
}
```

**5、测试一下：发现除了首页都进不去了！因为我们目前没有登录的角色，因为请求需要登录的角色拥有对应的权限才可以！**

![image-20220624103132841](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220624103132841.png)

**6、在configure()方法中加入以下配置，开启自动配置的登录功能！**

```java
// 开启自动配置的登录功能
// /login 请求来到登录页
// /login?error 重定向到这里表示登录失败
http.formLogin();
```

**7、测试一下：发现，没有权限的时候，会跳转到登录的页面！**

![image-20220624103203169](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220624103203169.png)

**8、查看刚才登录页的注释信息；**

<font color=red>**我们可以定义认证规则，重写configure(AuthenticationManagerBuilder auth)方法**</font>

```java
//定义认证规则
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
   
   //在内存中定义，也可以在jdbc中去拿....
   auth.inMemoryAuthentication()
          .withUser("kuangshen").password("123456").roles("vip2","vip3")
          .and()
          .withUser("root").password("123456").roles("vip1","vip2","vip3")
          .and()
          .withUser("guest").password("123456").roles("vip1","vip2");
}
```

**9、测试，我们可以使用这些账号登录进行测试！发现会报错！**

There is no PasswordEncoder mapped for the id “null”

![image-20220624103307310](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220624103307310.png)

**10、原因，我们要将前端传过来的密码进行某种方式加密，否则就无法登录，修改代码**

```java
//定义认证规则
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
   //在内存中定义，也可以在jdbc中去拿....
   //Spring security 5.0中新增了多种加密方式，也改变了密码的格式。
   //要想我们的项目还能够正常登陆，需要修改一下configure中的代码。我们要将前端传过来的密码进行某种方式加密
   //spring security 官方推荐的是使用bcrypt加密方式。
   
   auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
          .withUser("kuangshen").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
          .and()
          .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3")
          .and()
          .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2");
}
```

**11、测试，发现，登录成功，并且每个角色只能访问自己认证下的规则！搞定**

​	

<font color=008000>**认证和授权模板**</font>

```java
package com.xleixz.config;

import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

@EnableWebSecurity // 开启WebSecurity模式
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // 定制请求的授权规则
        // 首页所有人可以访问
        http.authorizeRequests().antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("vip1")
                .antMatchers("/level2/**").hasRole("vip2")
                .antMatchers("/level3/**").hasRole("vip3");

        // 开启自动配置的登录功能
        // /login 请求来到登录页
        // /login?error 重定向到这里表示登录失败
        http.formLogin();
    }

    //定义认证规则
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        //在内存中定义，也可以在jdbc中去拿....
        //Spring security 5.0中新增了多种加密方式，也改变了密码的格式。
        //要想我们的项目还能够正常登陆，需要修改一下configure中的代码。我们要将前端传过来的密码进行某种方式加密
        //spring security 官方推荐的是使用bcrypt加密方式。

        auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
                .withUser("xleixz").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2", "vip3")
                .and()
                .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1", "vip2", "vip3")
                .and()
                .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1", "vip2");
    }
}
```

​	

## 11.2 权限控制和注销

**1、开启自动配置的注销的功能**

```java
//定制请求的授权规则
@Override
protected void configure(HttpSecurity http) throws Exception {
   //....
   //开启自动配置的注销的功能
      // /logout 注销请求
   http.logout();
}
```

**2、我们在前端，增加一个注销的按钮，index.html 导航栏中**

```html
<a class="item" th:href="@{/logout}">
   <i class="address card icon"></i> 注销
</a>
```

**3、我们可以去测试一下，登录成功后点击注销，发现注销完毕会跳转到登录页面！**

**4、但是，我们想让他注销成功后，依旧可以跳转到首页，该怎么处理呢？**

```java
// .logoutSuccessUrl("/"); 注销成功来到首页
http.logout().logoutSuccessUrl("/");
```

**5、测试，注销完毕后，发现跳转到首页OK**

<font color=DEB8876>6、添加2个需求：</font>

> 需求一：用户没有登录的时候，导航栏上只显示登录按钮，用户登录之后，导航栏可以显示登录的用户信息及注销按钮！
>
> 需求二：比如小雷这个用户，它只有 vip2，vip3功能，那么登录则只显示这两个功能，而vip1的功能菜单不显示！

<font color=red>**需要结合thymeleaf中的一些功能sec：authorize="isAuthenticated()":是否认证登录！来显示不同的页面**</font>

Maven依赖：

```xml
<!-- https://mvnrepository.com/artifact/org.thymeleaf.extras/thymeleaf-extras-springsecurity4 -->
<dependency>
   <groupId>org.thymeleaf.extras</groupId>
   <artifactId>thymeleaf-extras-springsecurity5</artifactId>
   <version>3.0.4.RELEASE</version>
</dependency>
```

**7、修改我们的 前端页面**

导入命名空间

```html
xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity5"
```

修改导航栏，增加认证判断

```html
<!--登录注销-->
<div class="right menu">

   <!--如果未登录-->
   <div sec:authorize="!isAuthenticated()">
       <a class="item" th:href="@{/login}">
           <i class="address card icon"></i> 登录
       </a>
   </div>

   <!--如果已登录-->
   <div sec:authorize="isAuthenticated()">
       <a class="item">
           <i class="address card icon"></i>
          用户名：<span sec:authentication="principal.username"></span>
          角色：<span sec:authentication="principal.authorities"></span>
       </a>
   </div>

   <div sec:authorize="isAuthenticated()">
       <a class="item" th:href="@{/logout}">
           <i class="address card icon"></i> 注销
       </a>
   </div>
</div>
```

**8、重启测试，我们可以登录试试看，登录成功后确实，显示了我们想要的页面；**

9、如果注销404了，就是因为它默认防止csrf跨站请求伪造，因为会产生安全问题，我们可以将请求改为post表单提交，或者在spring security中关闭csrf功能；我们试试：在 配置中增加 `http.csrf().disable();`

```java
http.csrf().disable();//关闭csrf功能:跨站请求伪造,默认只能通过post方式提交logout请求
http.logout().logoutSuccessUrl("/");
```

10、继续将下面的角色功能块认证完成！

<font color=green>**菜单根据用户的角色动态的实现**</font>

```html
<div sec:authorize="hasRole('vip2')">
</div>
```

```html
<!-- sec:authorize="hasRole('vip1')" -->
<div class="column" sec:authorize="hasRole('vip1')">
   <div class="ui raised segment">
       <div class="ui">
           <div class="content">
               <h5 class="content">Level 1</h5>
               <hr>
               <div><a th:href="@{/level1/1}"><i class="bullhorn icon"></i> Level-1-1</a></div>
               <div><a th:href="@{/level1/2}"><i class="bullhorn icon"></i> Level-1-2</a></div>
               <div><a th:href="@{/level1/3}"><i class="bullhorn icon"></i> Level-1-3</a></div>
           </div>
       </div>
   </div>
</div>

<div class="column" sec:authorize="hasRole('vip2')">
   <div class="ui raised segment">
       <div class="ui">
           <div class="content">
               <h5 class="content">Level 2</h5>
               <hr>
               <div><a th:href="@{/level2/1}"><i class="bullhorn icon"></i> Level-2-1</a></div>
               <div><a th:href="@{/level2/2}"><i class="bullhorn icon"></i> Level-2-2</a></div>
               <div><a th:href="@{/level2/3}"><i class="bullhorn icon"></i> Level-2-3</a></div>
           </div>
       </div>
   </div>
</div>

<div class="column" sec:authorize="hasRole('vip3')">
   <div class="ui raised segment">
       <div class="ui">
           <div class="content">
               <h5 class="content">Level 3</h5>
               <hr>
               <div><a th:href="@{/level3/1}"><i class="bullhorn icon"></i> Level-3-1</a></div>
               <div><a th:href="@{/level3/2}"><i class="bullhorn icon"></i> Level-3-2</a></div>
               <div><a th:href="@{/level3/3}"><i class="bullhorn icon"></i> Level-3-3</a></div>
           </div>
       </div>
   </div>
</div>
```

11、测试一下！

![admin](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/admin.gif)

![2](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/2.gif)

![3](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/3.gif)

**权限控制和注销搞定！**

​	

## 11.3 改造"记住我"功能

现在的情况，只要登录之后，关闭浏览器，再登录，就会重新登录，但是很多网站的情况，就是有一个记住密码的功能，开始实现。

1、开启记住我功能

```java
//定制请求的授权规则
@Override
protected void configure(HttpSecurity http) throws Exception {
//.................
   //记住我
   http.rememberMe();
}
```

2、我们再次启动项目测试一下，发现登录页多了一个"记住我"功能，我们登录之后关闭浏览器，然后重新打开浏览器访问，发现用户依旧存在！

**实现原理：**

查看浏览器的cookie

![图片](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/640)

3、我们点击注销的时候，可以发现，**spring security 自动删除了这个 cookie**

![图片](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/640)

4、<font color=green>**结论：登录成功后，将cookie发送给浏览器保存，以后登录带上这个cookie，只要通过检查就可以免登录了。如果点击注销，则会删除这个cookie，具体的原理在JavaWeb阶段！**</font>

​	

## 11.4 定制登录页

现在这个登录页面都是spring security 默认的，怎么样可以使用我们自己写的Login界面呢？

1、在刚才的登录页配置后面指定 loginpage

```java
http.formLogin().loginPage("/toLogin");
```

2、然后前端也需要指向我们自己定义的 login请求

```html
<a class="item" th:href="@{/toLogin}">
   <i class="address card icon"></i> 登录
</a>
```

3、我们登录，需要将这些信息发送到哪里，我们也需要配置，login.html 配置提交请求及方式，方式必须为post:

在 loginPage()源码中的注释上有写明：

![图片](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/640)

```html
<form th:action="@{/login}" method="post">
   <div class="field">
       <label>Username</label>
       <div class="ui left icon input">
           <input type="text" placeholder="Username" name="username">
           <i class="user icon"></i>
       </div>
   </div>
   <div class="field">
       <label>Password</label>
       <div class="ui left icon input">
           <input type="password" name="password">
           <i class="lock icon"></i>
       </div>
   </div>
   <input type="submit" class="ui blue submit button"/>
</form>
```

4、这个请求提交上来，我们还需要验证处理，怎么做呢？我们可以查看formLogin()方法的源码！我们配置接收登录的用户名和密码的参数！

```java
http.formLogin()
  .usernameParameter("username")
  .passwordParameter("password")
  .loginPage("/toLogin")
  .loginProcessingUrl("/login"); // 登陆表单提交请求
```

5、在登录页增加记住我的多选框

```html
<input type="checkbox" name="remember"> 记住我
```

6、后端验证处理！

```java
//定制记住我的参数！
http.rememberMe().rememberMeParameter("remember");
```

7、测试，OK

​	

## 11.5 【Spring Security模板】完整配置代码

```java
package com.xleixz.config;

import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    //定制请求的授权规则
    @Override
    protected void configure(HttpSecurity http) throws Exception {

        http.authorizeRequests().antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("vip1")
                .antMatchers("/level2/**").hasRole("vip2")
                .antMatchers("/level3/**").hasRole("vip3");


        //开启自动配置的登录功能：如果没有权限，就会跳转到登录页面！
        // /login 请求来到登录页
        // /login?error 重定向到这里表示登录失败
        http.formLogin()
                .usernameParameter("username")
                .passwordParameter("password")
                .loginPage("/toLogin")
                .loginProcessingUrl("/login"); // 登陆表单提交请求

        //开启自动配置的注销的功能
        // /logout 注销请求
        // .logoutSuccessUrl("/"); 注销成功来到首页

        http.csrf().disable();//关闭csrf功能:跨站请求伪造,默认只能通过post方式提交logout请求
        http.logout().logoutSuccessUrl("/");

        //记住我
        http.rememberMe().rememberMeParameter("remember");
    }

    //定义认证规则
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        //在内存中定义，也可以在jdbc中去拿....
        //Spring security 5.0中新增了多种加密方式，也改变了密码的格式。
        //要想我们的项目还能够正常登陆，需要修改一下configure中的代码。我们要将前端传过来的密码进行某种方式加密
        //spring security 官方推荐的是使用bcrypt加密方式。

        auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
                .withUser("kuangshen").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2", "vip3")
                .and()
                .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1", "vip2", "vip3")
                .and()
                .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1", "vip2");
    }
}
```

