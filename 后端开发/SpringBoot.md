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

# 7、





