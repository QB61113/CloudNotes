# session

## 一、理解session

- [x] session：会话
- [x] 一次会话：用户打开浏览器，在进行一些列操作之后，最终将浏览器关闭，这个过程叫做一次会话。会话在服务器端有一个java对象，这个对象叫做session
- [x] 一次请求：用户在浏览器上点击一下，然后在页面停了下来，可以粗略认为是一次请求，请求对应的服务器端的对象叫做request
- [x] 一次会话对应N次请求（一次会话包含多个请求）
- [x] 为什么要session对象来保存会话状态？
  - 因为Http协议是一种无状态协议
    - 无状态：请求的时候，B和S是连接的，但是请求结束之后，连接就断开了（为了减轻服务器的压力）
  - 只要B和S断开了，==关闭浏览器这个动作，服务器是不知道的==（关闭浏览器和退出账号是不一样的，一个安全，一个不安全）
- [x] session是以cookie形式保存在浏览器中

​	

## 二、语法规范

- [x] 在java的Servlet规范中，session对应的类名是：`HttpSession(jakarta.servlet.http.HttpSession)`
- [x] 获取session对象
  - `HttpSession session = request_._getSession();`
- [x] 向会话域中绑定数据
  - `session.setAttribute();`
- [x] 从会话域取出数据
  - `Object obj = session.getAttribute();`

​	

## 三、session实现原理

✅session列表是一个Map，map的key是sessionid，map的value是session对象

✅用户发送第一次请求，服务器生成session对象，同时生成id，将id发送给浏览器

✅用户发送第二次请求，自动将浏览器内存中的id发送给服务器，服务器根据id找到session对象

✅关闭浏览器，内存小时，cookie消失，sessionid消失，会话等同于结束

​	

⚠️cookie禁用了，session还能找到吗？

✅cookie禁用：服务器正常发送cookie给浏览器，但是浏览器不要了，拒收了，并不是服务器不发了

✅cookie禁用了，session机制仍然可以实现，需要使用URL重启机制

- URL重写会增高开发者的成本，开发者在编写任何请求路径的时候，都需要在后面添加一个sessionid

​	

## 四、session对象的获取

✅获取session对象

`HttpSession session = request*.*getSession();`

作用：从WEB服务器当中获取session对象，如果session对象没有，则新建

✅seesion不新建对象

`HttpSession session = request*.*getSession(false);`

作用：从WEB服务器中获取session对象，如果session对象没有，则返回一个null

​	

### 4.1 session什么时候被销毁？

⚠️浏览器关闭的时候，浏览器服务器是不知道的

✅一种销毁：是超时销毁（超时机制）

✅一种销毁：是手动销毁（点击退出按钮）`session*.*invalidate();`

​	

### 4.2 超时机制

✅在web.xml中设置session超时时长

⚠️如果不配，默认是30分钟，设置路径在：Apache-tomcat/conf/web.xml中第638行

```xml
 <!--session的超时时长是30分钟-->
    <!--如果30分钟过去了，session对象仍然没有被访问，session会被销毁-->
    <session-config>
        <session-timeout>30</session-timeout>
    </session-config>
    
```

![20220425003558](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425003558.png)

​	

###  4.3 注意

✅JSP会在程序启动时，创建session对象，使得session对象不为空，可以在inedx.jsp中设置，使得JSP不创建seesion，但是不影响seesion的功能

![20220425003613](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220425003613.png)

​	

### 4.4 域对象的总结

✅request（对应的类名：HttpServletRequest）

- 请求域（请求级别的）

✅session（对应的类名：HttpSession）

- 会话域（用户级别的）

✅application（对应的类名：ServletContext）

- 应用域（项目级别的，所有用户共享的）

------

▶️这三个域的大小关系

- request < session < application

------

▶️他们三个域对象都有以下三个公用的方法

- `setAttribute`（向域当中绑定数据）
- `getAttribute`（从域当中获取数据）
- `removeAttribute`（删除域当中的数据）

------

▶️使用原则：尽量使用小的域