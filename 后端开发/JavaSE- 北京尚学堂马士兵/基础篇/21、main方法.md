## main方法
程序的入口，在同一个类中，如果有多个方法，那么虚拟机就会识别main方法，从这个方法作为程序的入口

- main方法格式严格要求：
   - public static void main(String[] args){}
   - public static --->修饰符 ，暂时用这个 -->面向对象一章
   - void --->代表方法没有返回值 对应的类型void
   - main --->见名知意名字
   - String[] args  --->形参  ---》不确定因素

---

## 重载方法
程序中其他的方法也叫main方法时,构成了重载
```java
public class TestArray10{
    public static void main(String[] args){
                
        }
        public static void main(String str){
                
        }
}
```


