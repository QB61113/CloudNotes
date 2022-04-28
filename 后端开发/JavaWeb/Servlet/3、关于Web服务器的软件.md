# 关于Web服务器的软件

## 一、Web服务器软件有哪些？

- Tomcat（Apache）
- jetty（Web服务器）
- JBoss（应用服务器）

### 1.1 应用服务器和Web服务器的关系

- 应用服务器实现了javaEE的所有规范
- Web服务器只实现了javaEE中的Servlet＋JSP两个核心的规范

## 二、Tomcat

### 2.1 下载

- [Apache官网](https://www.apache.org/)
- [Tomcat官网](https://tomcat.apache.org/)
- Tomcat开源免费的轻量级Web服务器
- Tomcat是java写的
- Tomcat服务器想要运行，必须先有jre（java运行时的环境）

### 2.2 Tomcat安装与配置

1. 解压安装
1. 启动Tomcat
   1. bin目录下有一个文件：startup.bat文件，通过他可以启动Tomcat服务器
   1. ...bat文件是Windows操作系统专用的，bat文件是批处理文件，这种文件可以编写大量的Windows的dos命令，然后执行bat文件就相当于在执行dos命令
   1. start.sh 这个文件在Windows中无法执行，他是在Linux中执行
3. 关于Tomcat服务器
   1. bii：Tomcat服务器的命令文件存放的目录，比如启动Tomcat
   1. conf：Tomcat服务器的配置文件存放目录（Servlet.xml文件可以配置端口号，默认Tomcat端口号是8080）
   1. lib：Tomcat服务器的核心程序目录，因为Tomcat服务器是java语言编写的，这里的jar包都是class文件
   1. logs：Tomcat服务器的日志目录，Tomcat服务器启动等信息都会在该目录下生成日志文件
   1. temp：Tomcat服务器的临时目录，存放临时文件
   1. webapps：这个目录是用来存放大量的webapp应用
   1. work：用来存放JSP文件翻译之后的class文件

