# 调试Debug在开发中大量应用
[TOC]

---

## 作用

> 可以有效地看到程序运行的步骤，可以看到程序一步一步运行数据的变化。

## 断点调试按钮解释
步骤

> 打断点------->Debug

![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425152244.png)一步一步的向下运行代码，不会走入任何方法中。



![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425152302.png)一步一步的向下运行代码，不会走入系统类库的方法中，但是会走入自定义的方法中。



![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425152309.png)一步一步的向下运行代码，会走入系统类库的方法中，也会走入自定义的方法中。



![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425152318.png)跳出方法



![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425152329.png)结束程序



![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425152337.png)进入到下一个断点，如果没有下一个断点了，就直接运行到程序结束。



![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425152344.png)在当前次取消未执行的断点。

## Debug的优化设置：更加节省内存空间
Debug的优化设置：更加节省内存空间：
> 设置Debug连接方式，默认是Socket。 Shared memory是Windows 特有的一个属性，一般在Windows系统下建议使用此设置，
> 内存占用相对较少。

![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425152455.png)

## 条件判断，查看表达式的值

### 条件判断

说明

> 调试的时候，在循环里增加条件判断，可以极大的提高效率，心情也能惧悦。  

具体操作

> 在断点处右击调出条件断点。可以在满足某个条件下，实施断点。  

![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425152547.png)

### 查看表达式的值

> 选择行，alt+f8。

![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425152611.png)
