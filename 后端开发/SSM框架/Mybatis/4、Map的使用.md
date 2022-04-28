# Map的使用

## 一、 Map

假设，实体类或数据库中的表，字段或者参数过多，应当考虑使用**Map**！
```java
   //万能的Map
    int addUser2(Map<String, Object> map);
```
```xml
  <!--对象中的属性可以直接取出来
    传递Map的key-->
    <insert id="addUser2" parameterType="map">
        insert into mybatis.user(id, name, pwd)
        values (#{userid}, #{username}, #{password})
    </insert>
```
```java
@Test
    public void addUser2() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        Map<String, Object> map = new HashMap<String,Object>();
        map.put("userid",5);
        map.put("username","Hello");
        map.put("password",222333);

        mapper.addUser2(map);

        //提交事务
        sqlSession.commit();

        //关闭SqlSession
        sqlSession.close();

    }
```

**Map**传递参数，直接在SQL中取出key即可！`parameterType="map"`
**对象**传递参数，直接在SQL中取出对象的属性即可！`parameterType="Object"`
只有 **一个基本类型**参数的情况，直接在SQL中取到！  **省略不写**

**多个参数用Map！，或注解！**
