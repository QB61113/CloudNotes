> final可以修饰变量、方法、类

# final修饰变量
#### 修饰基本数据类型
final修饰一个变量，变量的值不可以改变，这个变量也变成了一个字符常量，约定俗称的规定：名字大写

![20220427190815](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427190815.png)

#### 修饰引用数据类型
final修饰引用数据类型，那么地址值就不可以改变

![20220427190821](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427190821.png)

对象的属性依然可以改变

![20220427190826](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427190826.png)

---

# final修饰方法
当final修饰方法，那么这个方法不可以被该类的子类重写

![20220427190832](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427190832.png)

---

# final修饰类
final修饰类，代表没有子类，该类不可以被继承

一旦一个类被final修饰，那么里面的方法也没有必要用final修饰了（final可以省略不写）

![20220427190839](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427190839.png)

---

# Math类

1. 使用math类时，无需导包，直接可以使用
1. Math类没有子类，不能被其他类继承了

![20220427190849](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427190849.png)

3. 外界不可以创建对象

![20220427190928](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427190928.png)



![20220427190935](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427190935.png)

4. 发现Math类中的所有的属性，方法都被static修饰


那么不用创建对象去调用，只能通过类名.属性名  类名.方法名 去调用
