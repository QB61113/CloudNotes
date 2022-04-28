# request接口获取请求参数

- [x] 前端表单提交数据的时候，假设提交的是120这个数字，其实是以字符串"120"的方式提交到后端的，后端获取到的是字符串

## 一、测试request接口中的相关方法

```java
/**
* @description: 总结四个非常常用的方法
*
* Map<String,String[]> parameterMap = request.getParameterMap();
* Enumeration<String> names = request.getParameterNames();
* String [] values = request.getParameterValues("name");
* String value = request.getPatameter("name");
 */
```
## 二、各方法的详解

- [x] `Map<String,String[]> parameterMap = request.getParameterMap();`
- [x] 这个是获取Map
```java
/**
* @method Map<String,String[]> parameterMap = request.getParameterMap();
*/
//获取参数Map集合
Map<String, String[]> parameterMap = request.getParameterMap();

//遍历Map集合
//最基本的方式：获取Map集合中所有的Key，遍历
Set<String> keys = parameterMap.keySet();

//遍历Set集合
//迭代器
Iterator<String> it = keys.iterator();
//遍历
while (it.hasNext()) {
	String key = it.next();
	//System.out.println(key);
	
	//通过key获取value
	String[] values = parameterMap.get(key);
	//System.out.println(key + "=" + values);
	
	//遍历一维数组
	System.out.print(key + "=");
	
	for (String value : values) {
		System.out.print(value + ",");
	}
	//换行
	System.out.println();
        }
```

---

- [x] `Enumeration<String> names = request.getParameterNames();`
- [x] 这个是获取Map集合中的多有key
```java
/**
* @method Enumeration<String> names = request.getParameterNames();
* 直接通过getParameterNames()这个方法，
* 可以直接获取这个Map集合所有的key
*/
Enumeration<String> names = request.getHeaderNames();
while (names.hasMoreElements()) {
	String name = names.nextElement();
	//System.out.println("name");
	
        }
```

---

- [x] `String [] values = request.getParameterValues("name");`
- [x] 根据key获取Map集合的values
```java
/**
* @method String [] values = request.getParameterValues("name");
* 直接通过name获取values这个一位数组
*/
String[] usernames = request.getParameterValues("username");
String[] userpwds = request.getParameterValues("userpwd");
String[] interests = request.getParameterValues("interest");

//遍历一维数组
for (String username : usernames) {
	System.out.println(username);
}

for (String userpwd : userpwds) {
	System.out.println(userpwd);
}

for (String interest : interests) {
	System.out.println(interest);
        }
```

---

- [x] `String value = request.getPatameter("name");`
- [x] 获取value这个卫衣数组当中的第一个元素，这个方法最常用
```java
/**
* @method String value = request.getPatameter("name");
* 通过name获取value这个一维数组的第一个元素
* 这个方法使用的最多，因为这个一维数组一般只有一个元素
*/
String username = request.getParameter("username");
String userpwd = request.getParameter("userpwd");
//注意：这个方法只获取一维数组的第一个元素
//String interest = request.getParameter("interest");

//既然是CheckBox，调用方法：request.getParameterValues(:interest");
String[] interests02 = request.getParameterValues("interest");

System.out.println(username);
System.out.println(userpwd);
//System.out.println(interest);

for (String interest02 : interests02) {
	System.out.println(interest02);
}
    }
```

---

```java
package request;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.util.Enumeration;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

/**
* @Author: 小雷学长
* @Date: 2022/3/20 - 23:52
* @Version: 1.8
*/

/*
usename=zhangsan&userpwd=123&interest=s&interest=d
Map<String,String[]>
key            value
---------------------------
"username"      {"zhagnsan"}
"userpwd"       {"123"}
"interest"      {"s","d"}
*/

/**
* @description: 总结四个非常常用的方法
*
* Map<String,String[]> parameterMap = request.getParameterMap();
* Enumeration<String> names = request.getParameterNames();
* String [] values = request.getParameterValues("name");
* String value = request.getPatameter("name");
*/
public class requestTest extends HttpServlet {
	@Override
	protected void doPost(HttpServletRequest request, HttpServletResponse response)
		throws ServletException, IOException {
		
		
		//面向接口编程：HttpServletRequest接口
		//获取前端提交的数据
		//前端会提交
		// usename=zhangsan&userpwd=123&interest=s&interest=d
		//以上的数据会被小猫咪封装到request对象中
		
		/**
		* @method Map<String,String[]> parameterMap = request.getParameterMap();
		*/
		//获取参数Map集合
		Map<String, String[]> parameterMap = request.getParameterMap();
		
		//遍历Map集合
		//最基本的方式：获取Map集合中所有的Key，遍历
		Set<String> keys = parameterMap.keySet();
		
		//遍历Set集合
		//迭代器
		Iterator<String> it = keys.iterator();
		//遍历
		while (it.hasNext()) {
			String key = it.next();
			//System.out.println(key);
			
			//通过key获取value
			String[] values = parameterMap.get(key);
			//System.out.println(key + "=" + values);
			
			//遍历一维数组
			System.out.print(key + "=");
			
			for (String value : values) {
				System.out.print(value + ",");
			}
			//换行
			System.out.println();
		}
		
		System.out.println("----------------------");
		
		/**
		* @method Enumeration<String> names = request.getParameterNames();
		* 直接通过getParameterNames()这个方法，
		* 可以直接获取这个Map集合所有的key
		*/
		Enumeration<String> names = request.getHeaderNames();
		while (names.hasMoreElements()) {
			String name = names.nextElement();
			//System.out.println("name");
			
		}
		
		System.out.println("----------------------");
		
		/**
		* @method String [] values = request.getParameterValues("name");
		* 直接通过name获取values这个一位数组
		*/
		String[] usernames = request.getParameterValues("username");
		String[] userpwds = request.getParameterValues("userpwd");
		String[] interests = request.getParameterValues("interest");
		
		//遍历一维数组
		for (String username : usernames) {
			System.out.println(username);
		}
		
		for (String userpwd : userpwds) {
			System.out.println(userpwd);
		}
		
		for (String interest : interests) {
			System.out.println(interest);
		}
		
		System.out.println("----------------------");
		
		/**
		* @method String value = request.getPatameter("name");
		* 通过name获取value这个一维数组的第一个元素
		* 这个方法使用的最多，因为这个一维数组一般只有一个元素
		*/
		String username = request.getParameter("username");
		String userpwd = request.getParameter("userpwd");
		//注意：这个方法只获取一维数组的第一个元素
		//String interest = request.getParameter("interest");
		
		//既然是CheckBox，调用方法：request.getParameterValues(:interest");
		String[] interests02 = request.getParameterValues("interest");
		
		System.out.println(username);
		System.out.println(userpwd);
		//System.out.println(interest);
		
		for (String interest02 : interests02) {
			System.out.println(interest02);
		}
	}
}

```
