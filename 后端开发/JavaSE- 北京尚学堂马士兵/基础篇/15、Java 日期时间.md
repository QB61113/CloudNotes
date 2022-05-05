> java.util 包提供了 Date 类来封装当前的日期和时间。 Date 类提供两个构造函数来实例化 Date 对象。

- 第一个构造函数使用当前日期和时间来初始化对象。

`Date( )`

- 第二个构造函数接收一个参数，该参数是从1970年1月1日起的毫秒数。

`Date(long millisec)`
Date对象创建以后，可以调用下面的方法。

| 序号 | 方法和描述 |
| :--- | :--- |
| 1 | **boolean after(Date date)**
若当调用此方法的Date对象在指定日期之后返回true,否则返回false。 |
| 2 | **boolean before(Date date)**
若当调用此方法的Date对象在指定日期之前返回true,否则返回false。 |
| 3 | **Object clone( )**
返回此对象的副本。 |
| 4 | **int compareTo(Date date)**
比较当调用此方法的Date对象和指定日期。两者相等时候返回0。调用对象在指定日期之前则返回负数。调用对象在指定日期之后则返回正数。 |
| 5 | **int compareTo(Object obj)**
若obj是Date类型则操作等同于compareTo(Date) 。否则它抛出ClassCastException。 |
| 6 | **boolean equals(Object date)**
当调用此方法的Date对象和指定日期相等时候返回true,否则返回false。 |
| 7 | **long getTime( )**
返回自 1970 年 1 月 1 日 00:00:00 GMT 以来此 Date 对象表示的毫秒数。 |
| 8 | **int hashCode( )**
 返回此对象的哈希码值。 |
| 9 | **void setTime(long time)**
 
用自1970年1月1日00:00:00 GMT以后time毫秒数设置时间和日期。 |
| 10 | **String toString( )**
把此 Date 对象转换为以下形式的 String： dow mon dd hh:mm:ss zzz yyyy 其中： dow 是一周中的某一天 (Sun, Mon, Tue, Wed, Thu, Fri, Sat)。 |


---

## 获取当前日期时间
Java中获取当前日期和时间很简单，使用 Date 对象的 toString() 方法来打印当前日期和时间，
```java
import java.util.Date;
  
public class DateDemo {
   public static void main(String args[]) {
       // 初始化 Date 对象
       Date date = new Date();
        
       // 使用 toString() 函数显示日期时间
       System.out.println(date.toString());
   }
}

//run
Mon May 04 09:51:52 CDT 2013
```

---

## 日期比较
Java使用以下三种方法来比较两个日期：

- 使用 getTime() 方法获取两个日期（自1970年1月1日经历的毫秒数值），然后比较这两个值。
- 使用方法 before()，after() 和 equals()。例如，一个月的12号比18号早，则 new Date(99, 2, 12).before(new Date (99, 2, 18)) 返回true。
- 使用 compareTo() 方法，它是由 Comparable 接口定义的，Date 类实现了这个接口。

---

## 使用 SimpleDateFormat 格式化日期
SimpleDateFormat 是一个以语言环境敏感的方式来格式化和分析日期的类。SimpleDateFormat 允许你选择任何用户自定义日期时间格式来运行。例如：
```java
import  java.util.*;
import java.text.*;
 
public class DateDemo {
   public static void main(String args[]) {
 
      Date dNow = new Date( );
      SimpleDateFormat ft = new SimpleDateFormat ("yyyy-MM-dd hh:mm:ss");
 
      System.out.println("当前时间为: " + ft.format(dNow));
   }
}


```
> 这一行代码确立了转换的格式，其中 yyyy 是完整的公元年，MM 是月份，dd 是日期，HH:mm:ss 是时、分、秒。

**注意**:有的格式大写，有的格式小写，例如 MM 是月份，mm 是分；HH 是 24 小时制，而 hh 是 12 小时制。

---

## 日期和时间的格式化编码

时间模式字符串用来指定时间格式。在此模式中，所有的 ASCII 字母被保留为模式字母，定义如下：

| **字母** | **描述** | **示例** |
| :--- | :--- | :--- |
| G | 纪元标记 | AD |
| y | 四位年份 | 2001 |
| M | 月份 | July or 07 |
| d | 一个月的日期 | 10 |
| h |  A.M./P.M. (1~12)格式小时 | 12 |
| H | 一天中的小时 (0~23) | 22 |
| m | 分钟数 | 30 |
| s | 秒数 | 55 |
| S | 毫秒数 | 234 |
| E | 星期几 | Tuesday |
| D | 一年中的日子 | 360 |
| F | 一个月中第几周的周几 | 2 (second Wed. in July) |
| w | 一年中第几周 | 40 |
| W | 一个月中第几周 | 1 |
| a | A.M./P.M. 标记 | PM |
| k | 一天中的小时(1~24) | 24 |
| K |  A.M./P.M. (0~11)格式小时 | 10 |
| z | 时区 | Eastern Standard Time |
| ' | 文字定界符 | Delimiter |
| " | 单引号 | ` |


---

## 使用printf格式化日期
printf 方法可以很轻松地格式化时间和日期。使用两个字母格式，它以 **%t** 开头并且以下面表格中的一个字母结尾。

| 转  换  符 | 说    明 | 示    例 |
| :--- | :--- | :--- |
| c | 包括全部日期和时间信息 | 星期六 十月 27 14:21:20 CST 2007 |
| F | "年-月-日"格式 | 2007-10-27 |
| D | "月/日/年"格式 | 10/27/07 |
| r | "HH:MM:SS PM"格式（12时制） | 02:25:51 下午 |
| T | "HH:MM:SS"格式（24时制） | 14:28:16 |
| R | "HH:MM"格式（24时制） | 14:28 |

```java
import java.util.Date;
 
public class DateDemo {
 
  public static void main(String args[]) {
     // 初始化 Date 对象
     Date date = new Date();
 
     //c的使用  
    System.out.printf("全部日期和时间信息：%tc%n",date);          
    //f的使用  
    System.out.printf("年-月-日格式：%tF%n",date);  
    //d的使用  
    System.out.printf("月/日/年格式：%tD%n",date);  
    //r的使用  
    System.out.printf("HH:MM:SS PM格式（12时制）：%tr%n",date);  
    //t的使用  
    System.out.printf("HH:MM:SS格式（24时制）：%tT%n",date);  
    //R的使用  
    System.out.printf("HH:MM格式（24时制）：%tR",date);  
  }
}


//run

全部日期和时间信息：星期一 九月 10 10:43:36 CST 2012  
年-月-日格式：2012-09-10  
月/日/年格式：09/10/12  
HH:MM:SS PM格式（12时制）：10:43:36 上午  
HH:MM:SS格式（24时制）：10:43:36  
HH:MM格式（24时制）：10:43  
```
如果你需要重复提供日期，那么利用这种方式来格式化它的每一部分就有点复杂了。因此，可以利用一个格式化字符串指出要被格式化的参数的索引。
索引必须紧跟在%后面，而且必须以$结束。例如：
```java
import java.util.Date;
  
public class DateDemo {
 
   public static void main(String args[]) {
       // 初始化 Date 对象
       Date date = new Date();
        
       // 使用toString()显示日期和时间
       System.out.printf("%1$s %2$tB %2$td, %2$tY", 
                         "Due date:", date);
   }
}
```

---

## 解析字符串为时间
SimpleDateFormat 类有一些附加的方法，特别是parse()，它试图按照给定的SimpleDateFormat 对象的格式化存储来解析字符串。例如：
```java
import java.util.*;
import java.text.*;
  
public class DateDemo {
 
   public static void main(String args[]) {
      SimpleDateFormat ft = new SimpleDateFormat ("yyyy-MM-dd"); 
 
      String input = args.length == 0 ? "1818-11-11" : args[0]; 
 
      System.out.print(input + " Parses as "); 
 
      Date t; 
 
      try { 
          t = ft.parse(input); 
          System.out.println(t); 
      } catch (ParseException e) { 
          System.out.println("Unparseable using " + ft); 
      }
   }
}
```

---

## Java 休眠(sleep)
你可以让程序休眠一毫秒的时间或者到您的计算机的寿命长的任意段时间。例如，下面的程序会休眠3秒：
```java
import java.util.*;
  
public class SleepDemo {
   public static void main(String args[]) {
      try { 
         System.out.println(new Date( ) + "\n"); 
         Thread.sleep(1000*3);   // 休眠3秒
         System.out.println(new Date( ) + "\n"); 
      } catch (Exception e) { 
          System.out.println("Got an exception!"); 
      }
   }
}
```

---

## 测量时间
测量时间间隔（以毫秒为单位）：
```java
import java.util.*;
  
public class DiffDemo {
 
   public static void main(String args[]) {
      try {
         long start = System.currentTimeMillis( );
         System.out.println(new Date( ) + "\n");
         Thread.sleep(5*60*10);
         System.out.println(new Date( ) + "\n");
         long end = System.currentTimeMillis( );
         long diff = end - start;
         System.out.println("Difference is : " + diff);
      } catch (Exception e) {
         System.out.println("Got an exception!");
      }
   }
}
```

---

## Calendar类
Calendar类的功能要比Date类强大很多，而且在实现方式上也比Date类要复杂一些。
Calendar类是一个抽象类，在实际使用时实现特定的子类的对象，创建对象的过程对程序员来说是透明的，只需要使用getInstance方法创建即可。
### 创建一个代表系统当前日期的Calendar对象
`Calendar c = Calendar.getInstance();//默认是当前日期`
### 创建一个指定日期的Calendar对象
`//创建一个代表2009年6月12日的Calendar对象`
`Calendar c1 = Calendar.getInstance();`
`c1.set(2009, 6 -1 , 12)`;
### Calendar类对象字段类型
| 常量 | 描述 |
| :--- | :--- |
| Calendar.YEAR | 年份 |
| Calendar.MONTH | 月份 |
| Calendar.DATE | 日期 |
| Calendar.DAY_OF_MONTH | 日期，和上面的字段意义完全相同 |
| Calendar.HOUR | 12小时制的小时 |
| Calendar.HOUR_OF_DAY | 24小时制的小时 |
| Calendar.MINUTE | 分钟 |
| Calendar.SECOND | 秒 |
| Calendar.DAY_OF_WEEK | 星期几 |





