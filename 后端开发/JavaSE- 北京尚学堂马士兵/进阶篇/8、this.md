> this指代的就是当前对象


this关键字 用法
> - this可以修饰属性：
>    - 当属性名字和形参发生重名的时候，或者属性名字和局部变量重名的时候，都会发生就近原则
>    - 直接使用变量名字的话就指的是离的近的那个形参或者局部变量，
>    - 这时候如果想要表示属性的话，在前面要加上：this.修饰
>    - 如果不发生重名问题的话，要是访问属性也可以省略this.

![20220427190143](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427190143.png)

> - this修饰方法
>    - 在同一个类中，方法可以互相调用，this.可以省略不写

![20220427190149](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427190149.png)

> - this可以修饰构造器
>    - 同一个类中的构造器可以相互用this调用
>    - 注意：this修饰构造器必须放在第一行

![20220427190156](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427190156.png)

