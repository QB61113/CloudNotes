# Linux系统安装和卸载Redis

> 需要的工具：XShell、Xftp
>
> XShell 下载地址：[https://www.xshell.com/zh/xshell/](https://www.xshell.com/zh/xshell/ "点击下载 XShell")
>
> Xftp 下载地址：[https://www.xshell.com/zh/xftp/](https://www.xshell.com/zh/xftp/ "点击下载 Xftp")

​	

# 1 安装Redis

1、下载 Redis 安装包，下载地址：[Download | Redis](https://redis.io/download/ "点击下载 Redis")；

2、在连接 Xshell 后，使用 Xftp 工具将下载在本机上的安装包拖移到服务器（Linux系统）中；

3、解压压缩包 `tar -zxf redis-5.0.14.tar.gz`

4、切换到解压后的目录 `cd redis-5.0.14`

5、编译 `sudo make`  

> Linux中 `sudo`命令以系统管理者的身份执行指令

6、切换到 src 目录 `cd src`

7、执行安装 `sudo make install` 

​	

到这里安装Redis工作就结束了。但是安装过程中，没有设置安装位置，所以安装的位置为默认的位置。

​	

# 2 移动和配置

可以将可执行文件和配置文件移动到指定的目录：

1、切换到对应的目录（我的路径为/home/xl，可以根据自己的路径设置） `cd /home/xl`

2、创建对应的目录文件夹 `mkdir -p /home/xl/redis/bin`

3、切换到对应的目录 `cd /home/xl/redis-5.0.14`

4、移动**redis.conf**文件到对应的目录 `mv ./redis.conf /home/xl/redis/bin`

5、切换到src目录 `cd src`

6、移动 **mkreleasehdr.sh**，**redis-benchmark**，**redis-check-aof**，**redis-check-dump**，**redis-cli**，**redis-server**，**redis-sentinel** 到指定目录  `mv mkreleasehdr.sh redis-benchmark redis-check-aof redis-check-dump redis-cli redis-server redis-sentinel /home/xl/redis/bin`

> **比较重要的3个可执行文件**：
> redis-server：Redis服务器程序
> redis-cli：Redis客户端程序，它是一个命令行操作工具。也可以使用telnet根据其纯文本协议操作。
> redis-benchmark：Redis性能测试工具，测试Redis在你的系统及配置下的读写性能。

<font color="red">可能会找不到redis-check-dump，可以先忽略，服务启动后会自动生成。</font>

7、配置文件中 **daemonize** 是设置Redis服务是否需要一直在后台运行，若需要则把该项的值改为yes，不需要则no。

`sudo vi /home/xl/redis/bin/redis.conf` （vi 前加sudo 可以解决`:wq`保存时提示只读Readonly的问题），将daemonize no改为daemonize yes，`:wq`保存退出。

​	

# 3 启动Redis服务

启动redis并指定配置文件：

`/home/xl/redis/bin/redis-server`或 `cd /home/xl/redis/bin ./redis-server ./redis.conf` 

​	

查看redis是否启动成功：

`ps aux | grep redis` 

​		

查看主机的6379端口是否在使用（监听）：

`netstat -tlun`

​	

关闭redis服务器：

`pkill redis-server`

​	

# 4 卸载Redis

1、首先查看redis-server是否正在运行/启动：

`ps aux | grep redis`

2、关闭进程：

`kill -9 进程号`

3、删除redis相应的文件夹

`sudo rm -rf /home/xl/redis`

​	

# 5 报错问题

报错信息

```sh
In file included from server.c:31:0:
server.c:4999:59: error: ‘struct redisServer’ has no member named ‘cluster’
             (server.cluster_enabled && nodeIsMaster(server.cluster->myself)));
                                                           ^
cluster.h:58:27: note: in definition of macro ‘nodeIsMaster’
 #define nodeIsMaster(n) ((n)->flags & CLUSTER_NODE_MASTER)
                           ^
server.c: In function ‘main’:
server.c:5047:11: error: ‘struct redisServer’ has no member named ‘sentinel_mode’
     server.sentinel_mode = checkForSentinelMode(argc,argv);
           ^
server.c:5064:15: error: ‘struct redisServer’ has no member named ‘sentinel_mode’
     if (server.sentinel_mode) {
               ^
server.c:5131:19: error: ‘struct redisServer’ has no member named ‘sentinel_mode’
         if (server.sentinel_mode && configfile && *configfile == '-') {
                   ^
server.c:5153:168: error: ‘struct redisServer’ has no member named ‘sentinel_mode’
         serverLog(LL_WARNING, "Warning: no config file specified, using the default config. In order to specify a config file use %s /path/to/%s.conf", argv[0], server.sentinel_mode ? "sentinel" : "redis");
                                                                                                                                                                        ^
server.c:5158:11: error: ‘struct redisServer’ has no member named ‘supervised’
     server.supervised = redisIsSupervised(server.supervised_mode);
           ^
server.c:5158:49: error: ‘struct redisServer’ has no member named ‘supervised_mode’
     server.supervised = redisIsSupervised(server.supervised_mode);
                                                 ^
server.c:5159:28: error: ‘struct redisServer’ has no member named ‘daemonize’
     int background = server.daemonize && !server.supervised;
                            ^
server.c:5159:49: error: ‘struct redisServer’ has no member named ‘supervised’
     int background = server.daemonize && !server.supervised;
                                                 ^
server.c:5163:29: error: ‘struct redisServer’ has no member named ‘pidfile’
     if (background || server.pidfile) createPidFile();
                             ^
server.c:5168:16: error: ‘struct redisServer’ has no member named ‘sentinel_mode’
     if (!server.sentinel_mode) {
                ^
server.c:5178:19: error: ‘struct redisServer’ has no member named ‘cluster_enabled’
         if (server.cluster_enabled) {
                   ^
server.c:5186:19: error: ‘struct redisServer’ has no member named ‘ipfd_count’
         if (server.ipfd_count > 0 || server.tlsfd_count > 0)
                   ^
server.c:5186:44: error: ‘struct redisServer’ has no member named ‘tlsfd_count’
         if (server.ipfd_count > 0 || server.tlsfd_count > 0)
                                            ^
server.c:5188:19: error: ‘struct redisServer’ has no member named ‘sofd’
         if (server.sofd > 0)
                   ^
server.c:5189:94: error: ‘struct redisServer’ has no member named ‘unixsocket’
             serverLog(LL_NOTICE,"The server is now ready to accept connections at %s", server.unixsocket);
                                                                                              ^
server.c:5190:19: error: ‘struct redisServer’ has no member named ‘supervised_mode’
         if (server.supervised_mode == SUPERVISED_SYSTEMD) {
                   ^
server.c:5191:24: error: ‘struct redisServer’ has no member named ‘masterhost’
             if (!server.masterhost) {
                        ^
server.c:5201:19: error: ‘struct redisServer’ has no member named ‘supervised_mode’
         if (server.supervised_mode == SUPERVISED_SYSTEMD) {
                   ^
server.c:5208:15: error: ‘struct redisServer’ has no member named ‘maxmemory’
     if (server.maxmemory > 0 && server.maxmemory < 1024*1024) {
               ^
server.c:5208:39: error: ‘struct redisServer’ has no member named ‘maxmemory’
     if (server.maxmemory > 0 && server.maxmemory < 1024*1024) {
                                       ^
server.c:5209:176: error: ‘struct redisServer’ has no member named ‘maxmemory’
         serverLog(LL_WARNING,"WARNING: You specified a maxmemory value that is less than 1MB (current value is %llu bytes). Are you sure this is what you really want?", server.maxmemory);
                                                                                                                                                                                ^
server.c:5212:31: error: ‘struct redisServer’ has no member named ‘server_cpulist’
     redisSetCpuAffinity(server.server_cpulist);
                               ^
server.c: In function ‘hasActiveChildProcess’:
server.c:1480:1: warning: control reaches end of non-void function [-Wreturn-type]
 }
 ^
server.c: In function ‘allPersistenceDisabled’:
server.c:1486:1: warning: control reaches end of non-void function [-Wreturn-type]
 }
 ^
server.c: In function ‘writeCommandsDeniedByDiskError’:
server.c:3826:1: warning: control reaches end of non-void function [-Wreturn-type]
 }
 ^
server.c: In function ‘iAmMaster’:
server.c:5000:1: warning: control reaches end of non-void function [-Wreturn-type]
 }
 ^
make[1]: *** [server.o] Error 1
make[1]: Leaving directory `/opt/redis-6.0.6/src'
make: *** [all] Error 2

```

**原因**

自 redis 6.0.0 之后，编译 redis 需要支持 C11 特性，C11 特性在 4.9 中被引入。
Centos7 默认 gcc 版本为 4.8.5，所以需要升级gcc版本。

**解决**

```bash
yum -y install gcc gcc-c++ make tcl
yum -y install centos-release-scl
yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils scl enable devtoolset-9 bash 1234
```

升级之后问题解决。
