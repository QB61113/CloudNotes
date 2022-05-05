static（静态的）可以修饰
> - 属性
> - 方法
> - 代码块
> - 内部类


---

static修饰属性
```java
public class Test {
    //属性：
    int id;
    static int sid;
    //这是一个main方法，是程序的入口：
    public static void main(String[] args) {
        
        //创建一个Test类的具体的对象
        Test t1 = new Test();
        t1.id = 10;
        t1.sid = 10;
        
        Test t2 = new Test();
        t2.id = 20;
        t2.sid = 20;
        
        Test t3 = new Test();
        t3.id = 30;
        t3.sid = 30;
        
        //读取属性的值：
        System.out.println(t1.id);
        System.out.println(t2.id);
        System.out.println(t3.id);
        
        System.out.println(t1.sid);
        System.out.println(t2.sid);
        System.out.println(t3.sid);
    }
}
```
```java
10
20
30
 
30
30
30
```

一般官方的推荐访问方式：可以通过类名.属性名的方式去访问：
![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220427190441.png)

- static修饰属性总结
> 1. 在类加载的时候一起加载入方法区的静态域域中
> 1. 选于对象存在（优先存在）
> 1. 访问方式：
>    1. 对象名.属性名 
>    1. 类名.属性名（优先）      


static修饰属性的实例
> 数据的共享

![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220427190451.png)

属性
> 静态属性（类变量）
> 非静态属性（实例变量）



