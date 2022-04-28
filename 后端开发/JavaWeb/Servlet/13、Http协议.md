# Http协议

## 一、Http和Https理解

### 1.1 Http

- [x] HTTP（超文本协议）：是一个简单的请求-响应协议，它通常运行在TCP之上
   - 文本：html、字符串……
   - 超文本：图片、音乐、视频、定位、地图……
   - 80

### 1.2 Https理解

- [x] https：安全的
   - 443

## 二、两个时代

### 2.1 http1.0

- [x] HTTP/1.0
   - 客户端可以与web服务器连接后，只能获得一个web资源，断开连接

### 2.2 http1.1

- [x] HTTP/1.1
   - 客户端可以与web服务器连接后，可以获得多个web资源

## 三、HTTP请求

- [x] 客户端……发送请求（Request）……服务器
- [x] 以百度为例
```
Request URL:https://www.baidu.com/   请求地址
Request Method:GET     get方法/post方法
Status Code:200 OK     状态码：200
Remote(远程)  Address:14.215.177.39:443   地址+端口
Referrer Policy:no-referrer-when-downgrade
```
### 3.1 请求行

- [x] 请求行中的请求方式：`GET`
- [x] 请求方式：`Get`、`Post`

#### （1）Get请求方法

- [x] get：请求能够携带的参数比较少，大小有限制，会在浏览器的URL地址栏显示数据内容，不安全，但高效

[3.2 Get和Post方法](https://www.yuque.com/xleixz/aygur6/ssup8o?view=doc_embed)

#### （2）Post请求方法

- [x] post：请求能够携带的参数没有限制，大小没有限制，不会在浏览器的URL地址栏显示数据内容，安全，但不高效

[3.2 Get和Post方法](https://www.yuque.com/xleixz/aygur6/ssup8o?view=doc_embed)

### 3.2 请求头

```
Accept:告诉浏览器，它所支持的数据类型
Accept-Encoding：支持哪种格式  GBK  UTF-8  GB2312 ISO8859-1
Accept-Language：告诉浏览器，它的语言环境
Cache-Control：缓存控制
Connection:告诉浏览器。请求完成是断开还是保持连接
HOST：主机
…………
```
## 四、HTTP响应

```
Cache-Control:private    缓存控制
Connection:Keep-Alive    连接
Content-Encoding:gizp    编码
Content-Type:text/html   类型
```
### 4.1 响应体

```
Accept:告诉浏览器，它所支持的数据类型
Accept-Encoding：支持哪种格式  GBK  UTF-8  GB2312 ISO8859-1
Accept-Language：告诉浏览器，它的语言环境
Cache-Control：缓存控制
Connection:告诉浏览器。请求完成是断开还是保持连接
HOST：主机
…………
```
### 4.2 响应状态码

- [x] 200：请求成功
- [x] 3xx：请求重定向
   - 重定向：你重新到我给你的新位置去
- [x] 4xx：资源不存在
   - 404：找不到资源
- [x] 5xx：服务器代码错误（Java程序有问题）
   - 502：网关错误

