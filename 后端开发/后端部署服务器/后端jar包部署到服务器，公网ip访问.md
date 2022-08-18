# 后端jar包部署到服务器，公网ip访问

#### 1 检查服务器环境是否搭建完毕

> Linux系统安装和卸载Redis：[https://www.yuque.com/xleixz/backenddev/rg29go](https://www.yuque.com/xleixz/backenddev/rg29go "点击查看Linux系统安装和卸载 Redis")
>
> Linux系统安装MySQL：[https://www.yuque.com/xleixz/backenddev/xsi532](https://www.yuque.com/xleixz/backenddev/xsi532 "点击查看Linux系统安装MySQL")
>
> Linux系统安装Maven：[https://www.yuque.com/xleixz/backenddev/qsmrsw](https://www.yuque.com/xleixz/backenddev/qsmrsw "点击查看Linux系统安装Maven")
>
> Linux系统安装jdk：[https://www.yuque.com/xleixz/backenddev/zzm3ue](https://www.yuque.com/xleixz/backenddev/zzm3ue "点击查看Linux系统安装jdk")

#### 2 上传jar包到服务器

这里使用的工具为：**Xshell** 和 **Xftp**

> XShell 下载地址：https://www.xshell.com/zh/xshell/
>
> Xftp 下载地址：https://www.xshell.com/zh/xftp/

#### 3 启动jar包项目

<font color="red">**先切换到指定目录下：**</font>

```shell
cd /xxxx/xxxx/xx
```

三种启动方式：

- 启动程序，但`Ctrl+C`时会结束进程。

  ```shell
  java -jar xxxxx.jar
  ```

- 启动程序，不受继续输入命令影响，但到了一定时间会自动结束进程。

  ```shell
  java -jar xxxxx.jar &
  ```

- 永久启动，不结束进程，除非手动结束。

  ```shell
  nohup java -jar xxxxx.jar &
  ```

  会生成一个`nohup.out`文件，可以通过`bg`命令查看是否启动成功。

  ```shell
  bg
  ```

#### 4 查看进程

- 通过一下命令查看当前使用`java`命令启动的进程有哪些，检查是否启动成功。

  ```shell
  ps -ef|grep java|grep -v grep
  ```

  ![image-20220818161416133](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/查看Java进程命令.png)

- 若需要结束进程，可以通过一下命令结束进程（PID是进程号，如这里的75518）。

  ```shell
  kill -9 PID
  ```

**拓展：**

`根据进程pid查端口 和 根据端口port查进程`

```shell
# 根据进程pid查端口 
netstat -nap | grep pid 
# 根据端口port查进程 
netstat -nap | grep port
```

#### 5 设置防火墙，开放端口

> [基本常用防火墙设置端口命令]("点击查看基本常用防火墙设置端口命令")

- 通过命令查看已开放的端口。

  ```shell
  firewall-cmd --list-all
  ```

  ![image-20220818162029600](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/查看防火墙端口.png)

- 若没有则开放，若以开放就跳过。（如开放8085端口，port=后面就写8085）

  ```shell
  firewall-cmd --permanent --add-port=7001/tcp
  ```

- 重启防火墙后生效

  ```SHELL
  firewall-cmd --reload
  ```

此时通过`http://(你的ip地址):端口号`访问，有接口的可以直接访问接口`http://(你的ip地址):端口号/test/asd`

![image-20220818162532058](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/访问.png)

若访问不到，则需要设置一下服务器配置开放ip。

#### 6 服务器配置开放ip

以阿里云服务器ECS为例，进入**控制台**，选择左侧的选项卡中的**网络与安全**-**安全组**-**安全组规则**，选择快速添加。

![image-20220818163049042](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/添加阿里云网络组规则.png)

选择全部，点击确认后，选择刚添加的规则，点击编辑。

![image-20220818163239649](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/添加阿里云网络组规则2.png)

协议类型改成**全部**，保存即可。

![image-20220818163359963](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/添加阿里云网络组规则3.png)

通过`http://ip地址:端口号/`   就可以正常访问了。

![image-20220818163508314](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/正常访问.png)