包的作用
> - 为了解决重复名称的问题
> - 实际上包对应的就是盘符上的目录
> - 解决权限问题


创建包
> new → package

![20220427190617](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427190617.png)

包名定义
> 1. 名字全部小写
> 1. 中间用.隔开
> 1. 一般都是公司域名倒序写
>    1. com.jd   
> 4. 加上模块的名字
>    1. com.jd.login
> 5. 不能使用系统中的关键字
> 5. 包声明的位置一般都在非注释性代码的第一行


导包
> 进行定位

![20220427190624](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427190624.png)

总结
> 1. 使用不同包下的类需要导包
>    1. import java.util.Data;
> 2. 在导包以后，还想用其他包下同名的类，就必须要手动自己写所在的包
> 3. 同一个包下的类想使用不需要导包，可以直接使用
> 4. 在java.lang包下的类，可以直接使用无需导包
> 5. 手动导包
>    1. alt + enter
> 6. 自动导包

[IDEA的常用设置](https://www.yuque.com/xleixz/biancheng/eh7coc?view=doc_embed&inner=xxc8G)
> 7. 可以直接导入*
>    1. * 代表所有
> 8. 在Java中的导包没有包含和被包含的关系
>    1. 设置目录平级的格式（不是包含和被包含的显示）

[IDEA的常用设置](https://www.yuque.com/xleixz/biancheng/eh7coc?view=doc_embed&inner=s7Ctg)
> 9. 静态导入
>
>    ![20220427190722](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427190722.png)
>
> 10. 在静态导入后，同一个类中有相同的方法的时候，会优先走自己定义的方法
>
> ![20220427190737](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/20220427190737.png)

