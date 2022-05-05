- **大小写敏感**：Java 是大小写敏感的，这就意味着标识符 Hello 与 hello 是不同的。
- **类名**：对于所有的类来说，类名的首字母应该大写。如果类名由若干单词组成，那么每个单词的首字母应该大写，例如 **MyFirstJavaClass** 。
- **方法名**：所有的方法名都应该以小写字母开头。如果方法名含有若干单词，则后面的每个单词首字母大写。
- **源文件名**：源文件名必须和类名相同。当保存文件的时候，你应该使用类名作为文件名保存（切记 Java 是大小写敏感的），文件名的后缀为 **.java**。（如果文件名和类名不相同则会导致编译错误）。
- **主方法入口**：所有的 Java 程序由 **public static void main(String[] args)** 方法开始执行。

---

## Java 关键字

| 类别 | 关键字 | 说明 |
| :---: | :---: | :---: |
| 访问控制 | private | 私有的 |
|  | protected | 受保护的 |
|  | public | 公共的 |
|  | default | 默认 |
| 类、方法和变量修饰符 | abstract | 声明抽象 |
|  | class | 类 |
|  | extends | 扩充,继承 |
|  | final | 最终值,不可改变的 |
|  | implements | 实现（接口） |
|  | interface | 接口 |
|  | native | 本地，原生方法（非 Java 实现） |
|  | new | 新,创建 |
|  | static | 静态 |
|  | strictfp | 严格,精准 |
|  | synchronized | 线程,同步 |
|  | transient | 短暂 |
|  | volatile | 易失 |
| 程序控制语句 | break | 跳出循环 |
|  | case | 定义一个值以供 switch 选择 |
|  | continue | 继续 |
|  | default | 默认 |
|  | do | 运行 |
|  | else | 否则 |
|  | for | 循环 |
|  | if | 如果 |
|  | instanceof | 实例 |
|  | return | 返回 |
|  | switch | 根据值选择执行 |
|  | while | 循环 |
| 错误处理 | assert | 断言表达式是否为真 |
|  | catch | 捕捉异常 |
|  | finally | 有没有异常都执行 |
|  | throw | 抛出一个异常对象 |
|  | throws | 声明一个异常可能被抛出 |
|  | try | 捕获异常 |
| 包相关 | import | 引入 |
|  | package | 包 |
| 基本类型 | boolean | 布尔型 |
|  | byte | 字节型 |
|  | char | 字符型 |
|  | double | 双精度浮点 |
|  | float | 单精度浮点 |
|  | int | 整型 |
|  | long | 长整型 |
|  | short | 短整型 |
| 变量引用 | super | 父类,超类 |
|  | this | 本类 |
|  | void | 无返回值 |
| 保留关键字 | goto | 是关键字，但不能使用 |
|  | const | 是关键字，但不能使用 |
|  | null | 空 |


---

## Java注释
```java
public class HelloWorld {
   /* 这是第一个Java程序
    * 它将输出 Hello World
    * 这是一个多行注释的示例
    */
    public static void main(String[] args){
       // 这是单行注释的示例
       /* 这个也是单行注释的示例 */
       System.out.println("Hello World"); 
    }
}
```

---

## 继承

- 在 Java 中，一个类可以由其他类派生。如果你要创建一个类，而且已经存在一个类具有你所需要的属性或方法，那么你可以将新创建的类继承该类。
- 利用继承的方法，可以重用已存在类的方法和属性，而不用重写这些代码。被继承的类称为超类（super class），派生类称为子类（subclass）。
## 接口

- 在 Java 中，接口可理解为对象间相互通信的协议。接口在继承中扮演着很重要的角色。
- 接口只定义派生要用到的方法，但是方法的具体实现完全取决于派生类。

