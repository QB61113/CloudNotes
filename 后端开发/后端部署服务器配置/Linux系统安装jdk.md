# Linux系统安装jdk

> 需要的工具：XShell、Xftp
>
> XShell 下载地址：[https://www.xshell.com/zh/xshell/](https://www.xshell.com/zh/xshell/ "点击下载 XShell")
>
> Xftp 下载地址：[https://www.xshell.com/zh/xftp/](https://www.xshell.com/zh/xftp/ "点击下载 Xftp")

​	

# Xshell连接服务器并安装JDK

1、到Oracle官网下载需要的jdk版本到本机，jdk下载地址：[Java Downloads | Oracle](https://www.oracle.com/java/technologies/downloads/#java8 "点击到Oracle下载jdk")；

![image-20220725153717009](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220725153717009.png)

2、输入主机地址，通过XShell连接到服务器，根据提示输入账号和密码；

![image-20220725154654285](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220725154654285.png)

3、用`rz`命令将jdk上传到服务器；

```java
[root@xl ~]# rz
-bash: rz: 未找到命令
```

<font color='red'>没有安装rz的就先执行 `yum install lrzsz`</font>

```java
[root@xl ~]# yum install -y lrzsz
```

等待安装完毕后，继续用`rz`命令将jdk上传到服务器；

4、上传完毕后，解压jdk

`sudo tar -vxf jdk-8u341-linux-x64.tar.gz -home/xl`

5、配置环境变量 `sudo vim /etc/profile`，添加如下信息到配置文件中，注意jdk版本号和路径

```java
export JAVA_HOME=/home/xl/jdk1.8.0_341

export JRE_HOME=${JAVA_HOME}/jre

exportCLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib

export PATH=${JAVA_HOME}/bin:$PATH
```

> 按"i"编辑，编辑完成后按"esc"，接着输入":wq"保存退出

6、让配置文件生效 `source /etc/profile`

7、测试是否成功 `java -version`

```java
[root@xl ~]# source /etc/profile
[root@xl ~]# java -version
java version "1.8.0_341"
Java(TM) SE Runtime Environment (build 1.8.0_341-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.341-b13, mixed mode)
```

​	

出现版本号即配置成功，jdk也就配置完成了。

