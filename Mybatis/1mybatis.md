

<img src="file:///C:\Users\zzy13\Documents\Tencent Files\3035556390\nt_qq\nt_data\Pic\2025-07\Thumb\ebe436da679d4604656a67e3a99ffff8_720.jpg" alt="img" style="zoom: 33%;" />

一个mapper文件对应一个映射接口,映射多个实体bean



  mapper映射文件之间的划分

按业务模块划分,用户管理的sql用一个usermapper,订单管理相关的sql用ordermapper

按功能划分,所有的查询操作用querymapper,所有的更新操作用updatemapper









1.加载配置文件

2.构建SqlSessionFactory工厂

3.创建SqlSession对象,可以指定参数为true 表示事务自动提交(修改数据时不需要手动提交事务)

 SqlSession session = sqlSessionFactory.openSession(true);

4.执行映射配置文件的sql语句,并返回结果

```java
    public static void main(String[] args) throws Exception {

        InputStream inputStream = Resources.getResourceAsStream("MybatisConfig.xml");
//        InputStream inputStream1 = free.class.getClassLoader().getResourceAsStream("MybatisConfig.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        SqlSession session = sqlSessionFactory.openSession();
        List<user> list = session.selectList("user.selectall");

        for (user u : list) {
            u.information();
        }

        session.close();
        inputStream.close();

    }
```







映射配置文件(包括sql语句,并将返回的数据映射到指定类中resultType="org.example.springbootmybatis.user")

resultType指定返回结果的类型

parameterType指定参数类型

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">


<mapper namespace="user">
    <select id="selectall" resultType="org.example.springbootmybatis.user">
        select * from user
    </select>

    <select id="selectone" resultType="org.example.springbootmybatis.user" parameterType="java.lang.Integer">
        select * from user where id=#{id}
    </select>
</mapper>

```

核心配置文件

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="application.properties"/>
    引入配置文件后,可以用${}来使用配置文件里的参数
    
    <environments default="sql">  
    //default指定用那个环境,来连接不同数据库
    
        <environment id="sql">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${spring.datasource.driver-class-name}"/>
                <property name="url" value="${spring.datasource.url}"/>
                <property name="username" value="${spring.datasource.username}"/>
                <property name="password" value="${spring.datasource.password}"/>
            </dataSource>
        </environment>
    
    
      <environment id="sql2">  
    		.....
   	  </environment>
    
    </environments>
    
    
  
    <mappers>  //因为数据库中可以有多个表,mapper文件也需要映射多个
        <mapper resource="mapper.xml"/>
        <mapper resource="mapper1.xml"/>
    </mappers>
</configuration>
```





引入的数据库连接配置文件

```java
#下面这些内容是为了让MyBatis映射
#指定Mybatis的Mapper文件
mybatis.mapper-locations=classpath:mappers/*xml
#指定Mybatis的实体目录
mybatis.type-aliases-package=org.example.springbootmybatis.mybatis.entity

spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis
spring.datasource.username=root
spring.datasource.password=3035556390

```



数据映射的类

```java
package org.example.springbootmybatis;

public class user {
    private int id;
    private String username;
    private String password;
    private int age;

    public void information(){
        System.out.println(id+"\t"+username+"\t"+password+"\t"+age);

    }

    public int getId() {
        return id;
    }

    public String getUsername() {
        return username;
    }

    public String getPassword() {
        return password;
    }

    public int getAge() {
        return age;
    }
}

```

