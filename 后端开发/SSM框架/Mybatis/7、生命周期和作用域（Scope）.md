# 生命周期和作用域（Scope）

---

**生命周期**和**作用域**是至关重要的，因为错误的使用会导致非常严重的**并发问题**。  

程序运行流程：
![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424134434.png)

**SqlSessionFactoryBuilder：**

- 一旦创建了SqlSessionFactory，就不在需要它了。
- 局部变量。

**SqlSessionFactory：**

- 相当于_数据库连接池_。
- SqlSessionFactory一旦被创建就应该在应用的运行期间一直存在，**没有任何理由丢弃它或重新创建另一个实例**。
- 因此SqlSessionFactory的最佳作用域是应用作用域。
- 最简单的就是使用**单例模式**或静态单例模式。

**SqlSession**

- 连接到连接池的一个请求！
- SqlSession的实例不是线程安全的，因此是不能被共享的，所以他的最佳的作用域是请求或方法作用域。
- 用完之后需要赶紧**关闭**，否则资源被占用！_**这个操作是十分重要的。**_

![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424134447.png)
这里面的每一个Mapper，就代表一个具体的业务！

