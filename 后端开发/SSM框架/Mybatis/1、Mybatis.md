# Mybatis

---

**需要的关键知识**
JDBC、MySQL、JavaSE基础、Maven、Junit
**框架：**需要配置文件的。**最好的方式：**看官网的帮助文档。

## 一、Mybatis简介

### 1.1 什么是Mybatis

**什么是Mybatis：**Mybatis是一款优秀的**持久层框架**，它支持定制化SQL，存储过程以及高级映射（方面写SQL）。
**Mybatis**避免了几乎所有的JDBC代码和手动设置参数以及获取结果集。
**Mybatis**可以使用简单的xml或注解来配置和映射原生类型、接口和Java的POJO（Plain Old Objects，普通老式Java对象）为数据库中的记录。

**Mybatis**本是Apache的一个开源项目，2013年迁移到GitHub。



### 1.2 获取Mybatis

**Maven仓库：**[https://mvnrepository.com/](https://mvnrepository.com/)
```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.7</version>
</dependency>

```
![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424133612.png)



![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424133619.png)



![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424133652.png)



**GitHub仓库：**[https://github.com/](https://github.com/)
![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424133659.png)



![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424133716.png)



**中文帮助文档：**[https://github.com/tuguangquan/mybatis](https://github.com/tuguangquan/mybatis)



### 1.3 持久化

数据持久化
**持久化**就是将程序的数据在持久状态和瞬时状态转化的过程。
内存的特性：**断电即失。**



** 为什么需要持久化？**
有一些对象，不能让他丢掉。内存太贵。



### 1.4 持久层

**持久层**：完成持久化工作的代码块。**层**是界限十分明显的。



### 1.5 为什么需要Mybatis？

帮助程序员将数据存入数据库中。
特性：方便。传统的JDBC代码太复杂。简化，有现成的框架。自动化操作。
**使用的人多！！！！**
