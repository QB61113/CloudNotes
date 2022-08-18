# Linux系统安装MySQL

> 需要的工具：XShell、Xftp
>
> XShell 下载地址：[https://www.xshell.com/zh/xshell/](https://www.xshell.com/zh/xshell/ "点击下载 XShell")
>
> Xftp 下载地址：[https://www.xshell.com/zh/xftp/](https://www.xshell.com/zh/xftp/ "点击下载 Xftp")

<font color="red">**注意： MySQL8+ 与 MySQL8 以下版本安装步骤有所差异，根据版本需求对应不同的章节。**</font>

​	

# 1 MySQL8的安装

1、在 root 目录下，安装 `mysql` 和 `mysql-devel`

```sh
yum install mysql
yum install mysql-devel
```

2、安装`mysql-server`

```sh
wget http://dev.mysql.com/get/mysql80-community-release-el7-5.noarch.rpm
rpm -ivh mysql80-community-release-el7-5.noarch.rpm
yum install mysql-community-server
```

3、重启`mysql`服务

```sh
service mysqld restart
```

4、设置密码

```sh
# 登录mysql并输入密码
mysql -u root -p

# mysql8 修改密码方式
alter user 'root'@'localhost' identified by '这里填你要的密码';
```

> - mysql8初次安装后，需要先通过`cat /var/log/mysqld.log | grep password `命令查看密码，修改密码时，需要 **符合长度，且含有数字、小写或大写字母、特殊字符**
> - 无需重启数据库即可生效（且`mariadb`自动会被替换，不再生效）

5、进入 `/etc/my.cnf` 配置编码规则（无需配置的话，可跳过本步骤）

```sh
[mysql]
default-character-set =utf8
```

6、配置远程连接授权设置（配置后即可用navicat建立连接），至此完成安装！

```sh
# 如果要授权的用户是新用户，而不是root账户，则要先新建用户；如果要授权的是root用户，则跳过此命令
CREATE USER '这里填你要新建的账户'@localhost  IDENTIFIED BY '这里填要新建账户的密码';

# 授权，以root账户为例
GRANT ALL PRIVILEGES ON *.* TO 'root'@localhost WITH GRANT OPTION;

FLUSH PRIVILEGES;
```

7、使用 navicat 连接时报错

- 报错：`1045 - Access denied for user 'root'@'xxx'(using password: YES)`，解决方式见**上述步骤6**
- 报错：`1130 - Host 'xxx' is not allowed to connect to this MySQL server`，解决方式见下`第4点_过程遇到的问题`

​	

# 2 安装 MySQL8 以下版本

1. 在 root 目录下，安装 `mysql` 和 `mysql-devel`

   ```sh
   yum install mysql
   yum install mysql-devel
   ```

2. 安装`mysql-server`

   ```sh
   wget http://dev.mysql.com/get/mysql80-community-release-el7-5.noarch.rpm
   rpm -ivh mysql80-community-release-el7-5.noarch.rpm
   yum install mysql-community-server
   ```

3. 重启`mysql`服务

   ```sh
   service mysqld restart
   ```

4. 设置密码

   ```sh
   # 登录mysql并输入密码
   mysql -u root -p
   
   # mysql8 修改密码方式
   alter user 'root'@'localhost' identified by '这里填你要的密码';
   ```

   > - mysql7初次安装并登陆mysql时，root账户没有密码
   > - 无需重启数据库即可生效（且`mariadb`自动会被替换，不再生效）

5. 进入 `/etc/my.cnf` 配置编码规则（无需配置的话，可跳过本步骤）

   这里的字符编码必须和 `/usr/share/mysql/charsets/Index.xml` 中一致

   ```sh
   [mysql]
   default-character-set =utf8
   ```

6. 配置远程连接授权设置（配置后即可用navicat建立连接），至此完成安装！

   ```sh
   # 如果是新用户而不是root，则要先新建用户
   create user '这里填你要新建的用户名'@'%' identified by '这里填你要新建用户的密码'; 
   
   # 把在所有数据库的所有表的所有权限赋值给位于所有IP地址的root用户，以root账户为例
   grant all privileges on *.* to root@'%'identified by '这里填你的root账户密码';
   ```

​		

# 3 过程问题

1. 报错：`1130 - Host 'xxx' is not allowed to connect to this MySQL server`

   >  解决：1、执行登陆MySQL mysql -u root -p 密码 
   >
   > ​	  2、执行use mysql; 
   >
   > ​      3、执行update user set host = '%' where user = 'root'; 
   >
   > ​	  4、执行FLUSH PRIVILEGES; 

