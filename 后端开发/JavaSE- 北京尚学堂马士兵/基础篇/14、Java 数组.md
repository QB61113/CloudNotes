> Java 语言中提供的数组是用来存储固定大小的同类型元素。
> 你可以声明一个数组变量，如 numbers[100] 来代替直接声明 100 个独立变量 number0，number1，....，number99。


---

# 一维数组
> 案例



> 数组的学习

```java
/**
 * @Auther: 小雷学长
 * @Date: 2021/7/10 - 10:11
 * @Version: 12
 */
public class 数组的学习 {

    public static void main(String[] args) {
        /**
         * 数组的作用:用来存储相同类型的数据
         * 以int类型数据为例:数组用来存储int类型数据
         * 1.声明(定义数组)
         * 2.创建
         * 3.赋值
         * 4.使用
         */

        /*
        1.声明
        定义一个int类型的数组,名字叫arr
         */
        int[] arr;//int arr []

        /*
        2.创建
         */
        arr = new int[4];

        /*
        3.赋值
         */
        arr[0] = 12;
        arr[1] = 16;
        arr[2] = 23;
        arr[3] = 43;

        /*
        4.使用
         */
        System.out.println("arr:" + arr[1]);
        //获取数组的长度
        System.out.println("数组的长度为:" + arr.length);

    }
}

```


> 数组的引入

```java
import java.util.Scanner;

/**
 * @Auther: 小雷学长
 * @Date: 2021/7/8 - 21:35
 * @Version: 12
 */
public class 数组的引入 {

    public static void main(String[] args) {
        //功能:键盘录入是个学生的成绩,求平均数,求和

        Scanner in = new Scanner(System.in);

        //定义平均数变量
        int areaage = 0;

        //定义求和变量
        int sum = 0;
        //定义循环
        for (int i = 1; i <= 10; i++) {
            System.out.print("请输入第" + i + "个学生成绩:");
            int score = in.nextInt();
            sum += score;
            areaage = sum / 10;
        }
        System.out.println("学生成绩的和为:" + sum);
        System.out.println("学生成绩的平均数为:" + areaage);
    }
}


```


> 数组的引入_进阶

```java
import java.util.Scanner;

/**
 * @Auther: 小雷学长
 * @Date: 2021/7/10 - 10:38
 * @Version: 12
 */
public class 数组的引入_进阶 {
    public static void main(String[] args) {

        //定义一个int类型的数组,长度为10
        int[] scores = new int[10];
        //定义一个求和变量
        int sum = 0;
        Scanner in = new Scanner(System.in);

        for (int i = 1; i <= 10; i++) {
            System.out.print("请输入第" + i + "个学生成绩:");
            int score = in.nextInt();
            scores[i - 1] = score;
            sum += score;
        }
        System.out.println("\n10个学生成绩之和为:" + sum);
        System.out.println("10个学生成绩的平均数为:" + sum / 10);


        //求第几个学生的成绩
        System.out.print("\n请输入要查询的学生成绩:");
        int student = in.nextInt();
        int StudentScore = scores[student - 1];
        System.out.println("第" + student + "个学生成绩为:" + StudentScore);


    }
}

```


> 数组的最值问题

```java
import javax.swing.text.DefaultStyledDocument;

/**
 * @Auther: 小雷学长
 * @Date: 2021/7/14 - 10:58
 * @Version: 12
 */
public class 数组的最值问题 {
    /**
     * 实现一个功能:
     * 给定一个数组arr,求出数组中最大的数
     */
    public static void main(String[] args) {
        //定义数组arr
        int[] arr = {12, 3, 7, 4, 8, 125, 9, 45,7878};
        //假定最大的数为maxNum
        int maxNum = arr[0];
        //定义循环,使得每一个数组里的数都能与maxNum比较
        for (int i = 1; i < arr.length; i++) {
            if (maxNum < arr[i]) {
                maxNum = arr[i];
            }
        }
        System.out.println("数组里最大的数为:" + maxNum);
    }
}

```


> 利用方法求最值

```java
/**
 * @Auther: 小雷学长
 * @Date: 2021/7/14 - 11:23
 * @Version: 12
 */
public class 利用方法求最值 {
    public static void main(String[] args) {
        int[] arr = {12, 3, 7, 4, 8, 125, 9, 45, 7878, 9090};

        //使用getMaxNum(arr)调用方法并且接收maxNum返回值
        System.out.println("数组里最大的数为:" + getMaxNum(arr));
    }

    /**
     * @param arr
     * @return
     * @getMaxNum 定义方法
     */
    public static int getMaxNum(int[] arr) {
        int maxNum = arr[0];
        for (int i = 1; i < arr.length; i++) {
            if (maxNum < arr[i]) {
                maxNum = arr[i];
            }
        }
        //返回maxNum值
        return maxNum;
    }
}

```


> 数组的查询

```java
import java.util.Scanner;

/**
 * @Auther: 小雷学长
 * @Date: 2021/7/14 - 11:45
 * @Version: 12
 */
public class 数组的查询 {

    public static void main(String[] args) {
        /**
         * 查询指定位置的元素
         */
        int[] arr = {12, 34, 54, 76, 23, 56, 76}; //定义一个数组
        //查询指定位置的数组元素
        System.out.println("该数组里索引为3的元素为:" + arr[2]);

        System.out.println(("\n----------------\n"));

        /**
         * 查询指定元素对应的位置
         */
        //定义一个数组
        int[] arr2 = {12, 34, 54, 76, 23, 56, 76};

        Scanner in = new Scanner(System.in);
        System.out.print("12, 34, 54, 76, 23, 56, 76" +
                "\n请输入要查询的数索引:");
        int value = in.nextInt();
        int result = -1;
        //查询76对应的索引是多少
        for (int i = 0; i < arr2.length; i++) {
            if (arr2[i] == value) {
                result = i;
                break;
            }
        }
        if (result != -1) {

            System.out.println("76在数组里对应的的索引为:" + result);

        } else {

            System.out.println("输入的数不存在!");

        }

    }
}

```


> 数组的添加

```java
import javax.xml.transform.Source;
import java.util.Arrays;
import java.util.Scanner;

/**
 * @Auther: 小雷学长
 * @Date: 2021/7/18 - 17:06
 * @Version: 1.8
 */
public class 数组的添加 {
    public static void main(String[] args) {
        //定义数组
        int[] arr = {12, 3, 45, 65, 46, 67};
        System.out.print("添加前的数组为:" + Arrays.toString(arr));

        Scanner in = new Scanner(System.in);
        System.out.print("\n请输入要添加数的索引位置:");
        int index = in.nextInt();
        System.out.print("请输入需要添加的数:");
        int value = in.nextInt();

        /**
         * 用for循环方法
         *         arr[5] = arr[4];
         *         arr[4] = arr[3];
         *         arr[3] = arr[2];
         *         arr[2] = arr[1];
        */

        for (int i = arr.length - 1; i >= index + 1; i--) {
            arr[i] = arr[i - 1];

        }
        arr[index] = value;
        System.out.print("添加后的数组为:" + Arrays.toString(arr));

    }
}


```


> 数组的删除

```java
import java.util.Arrays;
import java.util.Scanner;

/**
 * @Auther: 小雷学长
 * @Date: 2021/7/18 - 16:24
 * @Version: 1.8
 */
public class 数组的删除 {
    public static void main(String[] args) {

        //定义数组     0  1  2  3   4
        int[] arr = {43, 76, 23, 99, 425}; //length=5

        Scanner in = new Scanner(System.in);
        System.out.print("请输入要删除的数组索引:");
        int value = in.nextInt();//value = 1

        /**
         * arr[1] = arr[2]
         * arr[2] = arr[3]
         * arr[3] = arr[4]
         * arr[4] = 0
         */

        for (int i = value; i <= arr.length - 1 - 1; i++) {
            arr[i] = arr[i + 1];
        }
        //数组的末尾设置0
        arr[arr.length - 1] = 0;
        System.out.println("删除后的数组为:" + Arrays.toString(arr));
    }
}
```

---

# 二维数组
## 二维数组的定义和遍历
```java
public class TestArray15{
        public static void main(String[] args){
                //定义一个二维数组：
                int[][] arr = new int[3][];//本质上定义了一个一维数组，长度为3
                
                int[] a1 = {1,2,3};
                arr[0] = a1;
                
                arr[1] = new int[]{4,5,6,7};
                
                arr[2] = new int[]{9,10};
                
                //读取6这个元素：
                //System.out.println(arr[1][2]);
                
                //对二维数组遍历：
                //方式1：外层普通for循环+内层普通for循环：
                for(int i=0;i<arr.length;i++){
                        for(int j=0;j<arr[i].length;j++){
                                System.out.print(arr[i][j]+"\t");
                        }
                        System.out.println();
                }
                
                //方式2：外层普通for循环+内层增强for循环：
                for(int i=0;i<arr.length;i++){
                        for(int num:arr[i]){
                                System.out.print(num+"\t");
                        }
                        System.out.println();
                }
                
                //方式3：外层增强for循环+内层增强for循环：
                for(int[] a:arr){
                        for(int num:a){
                                System.out.print(num+"\t");
                        }
                        System.out.println();
                }
                
                //方式4：外层增强for循环+内层普通for循环：
                for(int[] a:arr){
                        for(int i=0;i<a.length;i++){
                                System.out.print(a[i]+"\t");
                        }
                        System.out.println();
                }
        }
}

```

---

## 二维数组的初始化
> 静态初始化

`int [] [] arr = {{1,2,3},{4,5,6},{7,8,9}};`
`int [] [] arr = new int[] [] ``{{1,2,3},{4,5,6},{7,8,9}};`


> 动态初始化

```java
public class 二维数组 {
    public static void main(String[] args) {
        int [] [] arr = new int[3][];
        arr[0] = new int []{1,2};
        arr[1] = new int []{3,4};
        arr[2] = new int []{5,6};

        for (int []a:arr){
            for (int num:a){
                System.out.print(num+"\t");
            }
            System.out.println();
        }
    }
}
```
![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220427185646.png)

