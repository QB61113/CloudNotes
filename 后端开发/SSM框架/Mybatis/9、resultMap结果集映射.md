# resultMap结果集映射

> resultMap  结果集映射

```
id  name  pwd
id  name  password
```

结果集映射方法：

```xml
<!--id对应select中的resultMap的名字,type对应映射结果集-->
    <resultMap id="UserMap" type="User">

       <!--column数据中的字段，property实体类中的属性-->
       <!--相同的字段可以不写，只写需要映射的字段即可。
       <result column="id" property="id"/>
       <result column="name" property="name"/>
       -->
        <result column="pwd" property="password"/>
    </resultMap>

<!--使用resultMap类型-->
<select id="getUserById" resultMap="UserMap">
        select *
        from mybatis.user
        where id = #{id};
    </select>
```

- resultMap元素是 MyBatis 中最重要最强大的元素。  
- ResultMap的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。  
- ResultMap最优秀的地方在于，虽然你已经对它相当了解了，但是根本就不需要显示地用到他们。
   - 理解：字段一样的地方可以省略不写，只写需要映射的字段即可。

