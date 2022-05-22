# Maven的删除、安装、配置

[toc]

---

## 一、Maven删除

我们在Maven时通常会出现版本号与开发工具如（IDEA）版本不兼容的问题，这个时候需要我们更换Maven版本，删除maven需要删除本地仓库和相应的环境变量。

1. **删除整个maven文件夹，以我的为例，删除整个apache-maven-3.8.1**

![20220425220125](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425220125.png)

​	

2. **删除设置里的环境变量**

`MAVEN_HOME`

path里的`%MAVEN_HOME%\bin;`

![20220425220545](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425220545.png)

![20220425220632](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425220632.png)

**删除后一定要点击确定 ，否则会删除失败**

​	

3. **删除本地仓库**

如果之前配置过仓库路径，则删除之前的配置路径下的本地仓库，如下图就是我的仓库；

![20220425221008](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425221008.png)

如果没有配置过仓库地址，则默认在：`C:\Users\用户名\.m2\`路径下，删除名为`repository`的本地仓库。

![20220425221129](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425221129.png)

**到这里删除maven工作就完成了！**

​	

## 二、下载安装Maven

> 官网传送门：[https://maven.apache.org/](https://maven.apache.org/)

1. **点击Download到下载页面，可以下载最新版本的maven，也可以选择版本下载。（最新的不一定兼容，这里最好选择能兼容的版本号）。**

![20220425221643](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425221643.png)

​	

2. **在这里我选择的是3.8.1版本，如下图操作，点击apache-maven-3.8.1-bin.zip下载，等待下载解压后，下载工作就完成了！**

![20220425221918](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425221918.png)

![20220425222000](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425222000.png)

![20220425222031](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425222031.png)

​	

## 三、配置环境

1. 右击此电脑（我的电脑）----> 属性 ----> 高级系统设置 ----> 环境变量。

2. 在系统变量中点击**新建**

![20220425222837](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425222837.png)

编辑系统变量（以我的为例）：

```plain
变量名：   MAVEN_HOME
变量值：   你的maven解压的位置
```

![20220425222803](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425222803.png)

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425222535.png" alt="20220425222535" style="zoom:50%;" />

​	

3. 在系统变量的**Path**中点击**新建**，添加完后点击确定

```
%MAVEN_HOME%\bin
```

![20220425223153](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425223153.png)

​	

4. 打开命令行（win+R，输入cmd）查看是否配置成功

```
mvn -version
```

![20220425223343](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425223343.png)

​	

## 四、搭建本地仓库

> 在本地的仓库，远程仓库

1. 在本地新建一个文件夹，名字自取，作为本地仓库

![20220425223629](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425223629.png)

​		

2. 打开`apache-maven-3.8.1\conf`路径下的**settings.xml**配置文件，添加**localRepository**设置，仓库本地位置

![20220425224010](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425224010.png)

```xml
  <localRepository>D:\环境\apache-maven-3.8.1\myRepository</localRepository>
```

​	

## 五、阿里云镜像

- [x] 镜像：mirrors

  > 作用：加速下载

- [x] 国内建议使用阿里云的镜像

1. 将配置文件添加到`apache-maven-3.8.1\conf`下的**settings.xml**文件中

```xml
<mirror>
    <id>nexus-aliyun</id>
    <mirrorOf>*,!jeecg,!jeecg-snapshots</mirrorOf>
    <name>Nexus aliyun</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
<!--阿里云镜像-->
    <mirror>
	    <id>alimaven</id>
    	<mirrorOf>central</mirrorOf>
    	<name>aliyun maven</name>
	   <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
    </mirror>
```

![20220425225453](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425225453.png)

​	

## 六、从中央仓库下载需要的文件

1. 打开cmd，执行**mvn help:system**
2. 等待下载成功，这里下载较慢需耐心等待，出现**BUILD SUCCESS**则表示下载完成

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425225612.png" alt="20220425225612" style="zoom:50%;" />

​	

3. 仓库中会出现下载的东西，一切就绪！

<img src="https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425225833.png" alt="20220425225833" style="zoom:50%;" />

​	

到这里**Maven安装与配置工作就完成！**
