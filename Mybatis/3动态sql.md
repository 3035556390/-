sql-if

​           select * from user where id=? and username=?   if条件成立就加上,不成立就不加上

```java
    <select id="selectone" resultType="org.example.springbootmybatis.user" parameterType="org.example.springbootmybatis.user">
        select * from user
        <where>
            <if test="id != null">
                id=#{id}
            </if>
            <if test="username != null">
                and username= #{username}
            </if>
            <if test="password != null">
                and password= #{password}
            </if>
            <if test="age != null">
                and age= #{age}
            </if>
        </where>

    </select>
```



sql-foreach     

​     select * from user where id in (1,2,3)   其实就是对sql语句进行补全

​     select * from user  where   

​	 open="id in ("  表示以什么开始  id in(   

​	item    就是list中的元素  1,2,3

​	separator=","  元素间以,分隔

​	close=")"  最后补上右括号

```java
    <select id="selectById" resultType="org.example.springbootmybatis.user" parameterType="list">

        select * from user
        <where>
            <foreach collection="list" open="id in (" close=")" item="id" separator=",">
                #{id}
            </foreach>

        </where>
    </select>
```



sql片段的抽取和引用

使sql片段得到复用

```java
<mapper namespace="org.example.springbootmybatis.UserMapperInterface">
	
    <sql id="select">select * from user</sql>   //抽取


    <select id="selectall" resultType="org.example.springbootmybatis.User">
        <include refid="select"></include> 
    	//引用  refid指明复用哪个sql片段,如果后面还要跟上其它部分,如where什么直接在后面加就可以
    // <include refid="select"></include>  where id =#{id}  这样
    //就是一个sql语句的拼接
    
    </select>
    
    
    .....
 </mapper>   
```

