# pom文件



> pom是Maven的核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--Maven版本和头文件-->
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

  <!--这里就是我们刚才配置的GAV-->
    <groupId>com.xlxz</groupId>
    <artifactId>Javaweb-maven</artifactId>
    <version>1.0-SNAPSHOT</version>

  <!--Package:项目的打包方式
      jar:java应用
      war:Javaweb应用-->
  <packaging>war</packaging>
  
  <!--配置-->
      <properties>
        <!--项目的默认构建编码-->
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!--编码版本-->
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
  
  <!--项目依赖-->
  <dependencies>
      <!--具体依赖的jar包配置文件-->
  </dependencies>
  
  <!--项目构建用的东西-->
  <build>
    ………………
  </build>
    

</project>
```