![20220427183725](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427183725.png)
```java
/*定义上图的类*/
public class Dog {
    String breed;
    int size;
    String colour;
    int age;
 
    void eat() {
    }
 
    void run() {
    }
 
    void sleep(){
    }
 
    void name(){
    }
}
```
一个类可以包含以下类型变量：

- **局部变量**：在方法、构造方法或者语句块中定义的变量被称为局部变量。变量声明和初始化都是在方法中，方法结束后，变量就会自动销毁。
- **成员变量**：成员变量是定义在类中，方法体之外的变量。这种变量在创建对象的时候实例化。成员变量可以被类中方法、构造方法和特定类的语句块访问。
- **类变量**：类变量也声明在类中，方法体之外，但必须声明为 static 类型。

一个类可以拥有多个方法，在上面的例子中：eat()、run()、sleep() 和 name() 都是 Dog 类的方法。

---

## 构造方法

- 每个类都有构造方法。如果没有显式地为类定义构造方法，Java 编译器将会为该类提供一个默认构造方法。
- 在创建一个对象的时候，至少要调用一个构造方法。构造方法的名称必须与类同名，一个类可以有多个构造方法。

下面是一个构造方法示例：
```java
public class Puppy{
    public Puppy(){
    }
 
    public Puppy(String name){
        // 这个构造器仅有一个参数：name
    }
}
```

---

## 创建对象
对象是根据类创建的。在Java中，使用关键字 new 来创建一个新的对象。创建对象需要以下三步：

- **声明**：声明一个对象，包括对象名称和对象类型。
- **实例化**：使用关键字 new 来创建一个对象。
- **初始化**：使用 new 创建对象时，会调用构造方法初始化对象。
```java
public class Puppy{
   public Puppy(String name){
      //这个构造器仅有一个参数：name
      System.out.println("小狗的名字是 : " + name ); 
   }
   public static void main(String[] args){
      // 下面的语句将创建一个Puppy对象
      Puppy myPuppy = new Puppy( "tommy" );
   }
}
```

---

## 访问实例变量和方法
通过已创建的对象来访问成员变量和成员方法
```java
/*实例化对象*/
Object referenceVariable = new Constructor();
/*访问类中的变量*/
referenceVariable.variableName;
/*访问类中的方法*/
referenceVariable.mathodName;
```

---

## 实例
访问实例变量和调用成员方法
```java
public class Puppy{
    int puppyAge;
    public Puppy(String name){
        // 这个构造器仅有一个参数：name
        System.out.println("小狗的名字是 : " + name );//name为小狗的名字
    }

    public void setAge( int age ){
        puppyAge = age;//puppyAge为age 年龄
    }

    public void getAge( ){
        System.out.println("小狗的年龄为:" + puppyAge );
    }
//    public int getAge( ){
//        System.out.println("小狗的年龄为 : " + puppyAge );
//        return puppyAge;
//    }

    public static void main(String[] args){
        /* 创建对象 */
        Puppy myPuppy = new Puppy( "tommy" );
        /* 通过方法来设定age */
        myPuppy.setAge( 2);
        /* 调用另一个方法获取age */
        myPuppy.getAge( );
        /*你也可以像下面这样访问成员变量 */
        System.out.println("变量值 : " + myPuppy.puppyAge );
    }
}
```

---

## 源文件声明规则
> - 一个源文件中只能有一个 public 类
> - 一个源文件可以有多个非 public 类
> - 源文件的名称应该和 public 类的类名保持一致。例如：源文件中 public 类的类名是 Employee，那么源文件应该命名为Employee.java。
> - 如果一个类定义在某个包中，那么 package 语句应该在源文件的首行。
> - 如果源文件包含 import 语句，那么应该放在 package 语句和类定义之间。如果没有 package 语句，那么 import 语句应该在源文件中最前面。
> - import 语句和 package 语句对源文件中定义的所有类都有效。在同一源文件中，不能给不同的类不同的包声明。


---

## Java包
包主要用来对类和接口进行分类。当开发 Java 程序时，可能编写成百上千的类，因此很有必要对类和接口进行分类。
## import 语句
在 Java 中，如果给出一个完整的限定名，包括包名、类名，那么 Java 编译器就可以很容易地定位到源代码或者类。import 语句就是用来提供一个合理的路径，使得编译器可以找到某个类。

---

## 构造器
构造方法：
```java
public class Student {

    public String name;
    public int age;
    
    public void eat() {
        System.out.println("eat....");
    }
    
    //构造器
    /**
     * 名称与类名相同，没有返回值，不能写void
     * 构造器可以重载
     * 如果类中没有手动添加构造器，编译器会默认再添加一个无参构造器
     * 如果手动添加了构造器（无论什么形式），默认构造器就会消失
     */
    public Student() {
        System.out.println("无参构造器");
    }
    
    public Student(int a) {
        System.out.println("一个参数的构造器");
        age = 15;
    }
    
    public Student(int a, String s) {
        System.out.println("两个参数的构造器");
        age = a;
        name = s;
    }
}
```

## 一个简单的例子
在该例子中，我们创建两个类：**Employee** 和 **EmployeeTest**。
首先打开文本编辑器，把下面的代码粘贴进去。注意将文件保存为 Employee.java。
Employee 类有四个成员变量：name、age、designation 和 salary。该类显式声明了一个构造方法，该方法只有一个参数。
```java
import java.io.*;

public class Employee{
   String name;
   int age;
   String designation;
   double salary;
   // Employee 类的构造器
   public Employee(String name){
      this.name = name;
   }
   // 设置age的值
   public void empAge(int empAge){
      age =  empAge;
   }
   /* 设置designation的值*/
   public void empDesignation(String empDesig){
      designation = empDesig;
   }
   /* 设置salary的值*/
   public void empSalary(double empSalary){
      salary = empSalary;
   }
   /* 打印信息 */
   public void printEmployee(){
      System.out.println("名字:"+ name );
      System.out.println("年龄:" + age );
      System.out.println("职位:" + designation );
      System.out.println("薪水:" + salary);
   }
}
```
程序都是从main方法开始执行。为了能运行这个程序，必须包含main方法并且创建一个实例对象。
下面给出EmployeeTest类，该类实例化2个 Employee 类的实例，并调用方法设置变量的值。
```java
public static void main(String[] args){
        /* 使用构造器创建两个对象 */
        Employee empOne = new Employee("RUNOOB1");
        Employee empTwo = new Employee("RUNOOB2");

        // 调用这两个对象的成员方法
        empOne.empAge(26);
        empOne.empDesignation("高级程序员");
        empOne.empSalary(1000);
        empOne.printEmployee();

        empTwo.empAge(21);
        empTwo.empDesignation("菜鸟程序员");
        empTwo.empSalary(500);
        empTwo.printEmployee();
    }
}
```
合并：
```java
import java.io.*;

public class Employee {
    String name;
    int age;
    String designation;
    double salary;
    String marriage;

    // Employee 类的构造器
    public Employee(String name) {
        //this.name 为String name 的name
        this.name = name;
    }

    // 设置age的值
    public void Age(int Age) {
        age = Age;
    }

    /* 设置designation的值*/
    public void empDesignation(String empDesig) {
        designation = empDesig;
    }

    /* 设置salary的值*/
    public void empSalary(double empSalary) {
        salary = empSalary;
    }

    /*婚姻情况*/
    public void Marriage_status(String situation) {
        marriage = situation;
    }

    /* 打印信息 */
    public void printEmployee() {
        System.out.println("姓名:" + name);
        System.out.println("年龄:" + age);
        System.out.println("职位:" + designation);
        System.out.println("薪水:" + salary);
        System.out.println("婚姻情况:" + marriage);
        System.out.println("\n");
    }

    public static void main(String[] args) {
        /* 使用构造器创建两个对象 */
        Employee empOne = new Employee("雷小喜");
        Employee empTwo = new Employee("王明");
        Employee empThree = new Employee("夏雨");


        // 调用这两个对象的成员方法
        empOne.Age(22);
        empOne.empDesignation("1号程序员");
        empOne.empSalary(50000);
        empOne.Marriage_status("未婚");
        empOne.printEmployee();

        empTwo.Age(21);
        empTwo.empDesignation("2号程序员");
        empTwo.empSalary(10000);
        empTwo.Marriage_status("未婚");
        empTwo.printEmployee();

        empThree.Age(26);
        empThree.empDesignation("3号程序员");
        empThree.empSalary(12000);
        empThree.Marriage_status("已婚");
        empThree.printEmployee();


    }
}

```
