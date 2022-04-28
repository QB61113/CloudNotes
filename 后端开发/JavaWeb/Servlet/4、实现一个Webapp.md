# 实现一个Webapp

## 一、实现一个基本的Web应用

### 1.1 步骤

1. 找到CATALINA_HOME\webapps目录。因为所有的APP要放到该目录下，若不放，则找不到
1. 在CATALINA_HOME\webapps下新建一个子目录oa
   1. 这个oa就是这个webapp的名字
3. 在oa目录下新建资源。例如：index.html
   1. 编写html内容
4. 启动Tomcat服务器
4. 打开浏览器输入url
   1. http://127.0.0.1:8080/oa/index.html

### 2.2 思考

在浏览器上直接输入url动作和使用超链接是一样的
```html
<a href="http://127.0.0.1:8080/oa/login.html">user login</a>

<a href="/oa/login.html">user login</a>
```
## 二、静态资源变动态资源

- 连接数据库需要JDBC程序，也就是需要编写java程序连接数据库，网页内容随着数据库内的数据改变而改变
- 对于一个动态的Web来说，一个请求和响应的过程有多少个角色参与，角色与角色之间有多少个协议
