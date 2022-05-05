# toString
> 对数组进行遍历查看的

```java
 //给定一个数组：
        int[] arr = {1, 3, 7, 2, 4, 8};
        //toString:对数组进行遍历查看的，返回的是一个字符串，这个字符串比较好看
        System.out.println(Arrays.toString(arr));
```
![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220427185845.png)

---

# binarySearch
> 二分法查找：找出指定数组中的指定元素对应的索引

```java
int[] arr = {1, 3, 7, 2, 4, 8};
Arrays.sort(arr);
        System.out.println(Arrays.toString(arr));
        System.out.println(Arrays.binarySearch(arr, 4));
```
![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220427185934.png)

---

# sort
> 排序 -->升序

```java
int[] arr = {1, 3, 7, 2, 4, 8};
Arrays.sort(arr);
        System.out.println(Arrays.toString(arr));
       
```
![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220427185942.png)

---

# copyOf
> 完成数组的复制

```java
int[] arr2 = {1, 3, 7, 2, 4, 8};
        //copyOf:完成数组的复制：
        int[] newArr = Arrays.copyOf(arr2, 4);
        System.out.println(Arrays.toString(newArr));
```
![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220427185954.png)

---

# copyOfRange
> 区间复制

```java
int[] newArr2 = Arrays.copyOfRange(arr2, 1, 4);//[1,4)-->1,2,3位置
        System.out.println(Arrays.toString(newArr2));
```
![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220427190008.png)

---

# equals
> 比较两个数组的值是否一样

```java
int[] arr3 = {1, 3, 7, 2, 4, 8};
        int[] arr4 = {1, 3, 7, 2, 4, 8};
        System.out.println(Arrays.equals(arr3, arr4));//true
        System.out.println(arr3 == arr4);
//false ==比较左右两侧的值是否相等，比较的是左右的地址值，返回结果一定是false
```
![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220427190017.png)

---

# fill
> 数组的填充

```java
int[] arr5 = {1, 3, 7, 2, 4, 8};
        Arrays.fill(arr5, 10);
        System.out.println(Arrays.toString(arr5));
```
![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220427190031.png)
