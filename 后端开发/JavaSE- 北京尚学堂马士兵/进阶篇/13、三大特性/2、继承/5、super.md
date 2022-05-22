> super指的是父类的
> super可以修饰属性、方法



# super修饰属性、方法
_在子类方法中，可以通过_`_super._`_属性的方式，调用父类提供的属性、方法_
_通常情况下，_`super.`可以省略不写
![20220427191333](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427191333.png)

在特殊情况下，当子类与父类的属性、方法重名时，若要调用父类的属性、方法，必须加上修饰符`super.`来调用

# super修饰构造器
> 其实我们平时写的构造器的第一行都有super()
> 作用：调用父类构造器    
> *平时省略不写

在构造器中，super调用父类构造器和this调用子类构造器只能存在一个，两者不能共存
_因为super修饰构造器要放在第一行，this修饰构造器也要放在第一行    _
![20220427191343](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427191343.png)
解决：
_super和this只能存在一个，二选一即可_

- _IDEA快捷键生成构造器_
   - Alt + insert 
      - ![20220427191351](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427191351.png)

