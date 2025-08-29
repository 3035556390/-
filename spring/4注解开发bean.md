![image-20250706151848119](C:\Users\zzy13\AppData\Roaming\Typora\typora-user-images\image-20250706151954521.png)

![屏幕截图 2025-07-06 151844](C:\Users\zzy13\Pictures\Screenshots\屏幕截图 2025-07-06 151844.png)







纯注解开发

写一个配置类@Configuration,用@ComponentScan设定扫描路径,@PropertySource("jdbc.properties")加载properties文件

```java
@Configuration
@ComponentScan({"example.dao.impl","example.service.impl"})
@PropertySource("jdbc.properties")
public class configuration {

}

```

```java
ApplicationContext ctx3=new AnnotationConfigApplicationContext(configuration.class);
```







用注解定义bean的生命周期

```java
@Scope("prototype") //设定为非单列

```

```java
@PostConstruct  //初始操作,指的是容器创建时执行的操作
```

```java
@PreDestroy    //销毁操作 指的是容器销毁时执行的操作
```

