# Mybatis增删改查实现

重点：**增删改**需要**提交事务**

## 一、namespace

namespace中的包名要和Mapper接口的包名一致
![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424134307.png)

## 二、select

选择，查询语句。
**id**：就是对应的namespace中的方法名；
**resultType**：SQL语句执行的返回值；
**parameterType**：参数类型；

### 2.1 步骤详解

1. 编写接口；
```java
public interface UserMapper {
      //根据ID查询用户
    User getUserById(int id);
}
```

2. 编写对应的Mapper中对的SQL语句；
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace绑定一个对应的Dao/Mapper接口-->
<mapper namespace="xleixz.dao.UserMapper">
  
    <select id="getUserById" parameterType="int" resultType="xleixz.pojo.User">
        select *
        from mybatis.user
        where id = #{id}
    </select>
  </mapper>
```

3. 测试。(**增删改需要提交事务**）
```java
 //增删改需要提交事务
    //测试插入
    @Test
    public void addUser() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        mapper.addUser(new User(4, "小雷", "123456"));

        //提交事务
        sqlSession.commit();

        //关闭SqlSession
        sqlSession.close();

    }
```
## 三、insert

注意：**增删改需要提交事务**
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace绑定一个对应的Dao/Mapper接口-->
<mapper namespace="xleixz.dao.UserMapper">
  
    <!--对象中的属性可以直接取出来-->
    <insert id="addUser" parameterType="xleixz.pojo.User">
        insert into mybatis.user(id, name, pwd)
        values (#{id}, #{name}, #{pwd})
    </insert>
  </mapper>
```
## 四、update

注意：**增删改需要提交事务**
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace绑定一个对应的Dao/Mapper接口-->
<mapper namespace="xleixz.dao.UserMapper">
  
    <update id="updateUser" parameterType="xleixz.pojo.User">
        update mybatis.user
        set name = #{name},
            pwd  = #{pwd}
        where id = #{id}
    </update>
  </mapper>
```
## 五、delete

注意：**增删改需要提交事务**
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace绑定一个对应的Dao/Mapper接口-->
<mapper namespace="xleixz.dao.UserMapper">
  
<delete id="deleteUser" parameterType="int">
        delete from mybatis.user
        where id = #{id}
    </delete>
  </mapper>
```



## 六、错误分析

**标签**不能匹配错，标签必须相对应，插入就插入，查询就查询。

resources文件夹下的xml中，`mappers`标签中的**resource**值中的不是`.`，而是`/`符号。
```xml
<!--每一个Mapper.xml都需要在Mybatis核心配置文件中注册！！-->
    <mappers>
        <mapper resource="xleixz/dao/UserMapper.xml"/>
    </mappers>
```

程序配置文件，必须符合规则！！

![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220424134332.png)
`NullPointerException`**空指针异常**报错，原因是没有注册到资源！！

**输出的xml文件中存在中文乱码问题！**

**Maven资源无法导出问题**，解决---->在Maven中加入`build`，详情见[Maven资源无法导出](https://www.yuque.com/xleixz/ksbdf5/cu8qnq#Fus64)

