# Linux系统安装Maven

> 需要的工具：XShell、Xftp
>
> XShell 下载地址：[https://www.xshell.com/zh/xshell/](https://www.xshell.com/zh/xshell/ "点击下载 XShell")
>
> Xftp 下载地址：[https://www.xshell.com/zh/xftp/](https://www.xshell.com/zh/xftp/ "点击下载 Xftp")

​	

1. 到Maven官网下载对应的Maven版本，Maven下载地址：[Maven – Download Apache Maven](https://maven.apache.org/download.cgi "点击下载Maven")

   ![image-20220725163614711](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/Maven.png)

2. 可以通过XFtp工具将本地压缩包拖到服务器上

   ![image-20220725164151256](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/Maven-XShell.png)

3. 解压包

   ```sh
   tar -zxvf apache-maven-3.8.6-bin.tar.gz
   ```

4. 配置环境变量 `sudo vi /etc/profile`

   ```sh
   sudo vi /etc/profile
   
   # 最后追加这两行
   
   export MAVEN_HOME=/mydata/maven/apache-maven-3.8.6
   export PATH=$MAVEN_HOME/bin:$PATH
   
   # 重新加载配置文件
   
   source /etc/profile
   ```

5. `mvn -v` 可以看到如下输出就代表成功了

   ```sh
   [root@xl maven]# mvn -v
   Apache Maven 3.8.6 (cecedd343002696d0abb50b32b541b8a6ba2883f)
   Maven home: /mydata/maven/apache-maven-3.8.6
   Java version: 1.8.0_341, vendor: Red Hat, Inc., runtime: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.341.b10-1.el8_4.x86_64/jre
   Default locale: en_US, platform encoding: UTF-8
   OS name: "linux", version: "4.18.0-240.22.1.el8_3.x86_64", arch: "amd64", family: "unix"
   ```

6. 配置maven的**settings**文件，找到maven的安装路径下的**conf/settings.xml**文件，配置好localRepository(本地仓库)标签的路径

   ```sh
    <localRepository>/mydata/maven/repository</localRepository>
   ```

7. 配置镜像源地址

   ```sh
   <mirror>
         <id>alimaven</id>
         <name>aliyun maven</name>
          <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
         <mirrorOf>central</mirrorOf>
   </mirror>
   ```

   保存文件退出`:wq`

8. 查看是否成功 `mvn help:system`

   ![image-20220725164757071](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/Maven%E6%98%AF%E5%90%A6%E6%88%90%E5%8A%9F.png)

看到上图结果，则表示成功。

