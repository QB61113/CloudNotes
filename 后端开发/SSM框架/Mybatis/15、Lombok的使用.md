# Lombok的使用

> Lombok是一个可以通过简单的注解形式来帮助我们简化消除一些必须有但显得很臃肿的Java代码的工具，通过使用对应的注解，可以在编译源码的时候生成对应的方法。
>
> 理解：lombok插件用于“偷懒”，相当于Alt＋Insert快捷键，能自动生成无参构造，get、set、toString、hashcode、equals等方法。
>
> 缺点：缺少直观的代码，新手可能看不懂结构。等于看不到直观的get、set方法的代码。插件依赖jar包，没有jar包无法使用。
>
> 官网帮助文档传送门：https://projectlombok.org/

<img src="https://gitee.com/xleixz/cloud-notes/raw/master/Typora-Images/20220423202247.png" alt="image-20220423202243377" style="zoom:33%;" />



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

