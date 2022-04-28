# Request接口的详解

- [x] HttpServletRequest是一个接口，全限定名称：jakarta.servlet.http.HttpServletRequest
- [x] HttpServletRequest接口是Servlet规范中的一员
- [x] HttpServletRequest接口的父接口是：ServletRequest
   - `public interface HttpServletRequest extends ServletRequest{}`
- [x] HttpServletRequest接口的实现类是
- [x] Javaweb程序员面向HttpServletRequest接口编程，调用方法就可以获取到请求的信息了
- [x] 获取用户提交的数据的4个方法
```java
Map<String.String[]> getParameterMap() 这个是获取Map
Enumer ation<String> getParameterNames()  这个是获取Map集合中的多有key
String [] getParameter Values(java.lang.String name)  根据key获取Map集合的values
String getParameter(String name)  获取value这个一维数组当中的第一个元素，这个方法最常用
    //以上的4个方法和获取用户提交的数据有关系
```

- [x] `String getParameter(String name)`这个方法用的最多
