```java
<bean id="bookdao" class="example.dao.impl.BookDaoImpl"/>
<bean id="bookservice" name="service" class="example.service.impl.BookServiceImpl" scope="singleton">
<!--     name 和id一样,是id的别名,scope里的singleton是控制创建对象时同个对象, protptype创建时是不同对象  -->

```

```java
ApplicationContext applicationContext=new ClassPathXmlApplicationContext("applicationContext.xml");
BookServiceImpl bookServiceimpl =(BookServiceImpl)applicationContext.getBean("bookservice");


```



bean本质是对象,这个对象有四种方式,第四种最常用

```java

<!--    构造方法创建bean-->
    <bean id="bookservice" name="service" class="example.service.impl.BookServiceImpl" scope="singleton">
<!--     name 和id一样,是id的别名,scope里的singleton是控制创建对象时同个对象, protptype创建时是不同对象  -->

    </bean>


<!--    静态工厂创建bean-->
    <bean id="bookservice1" class="example.factory.ServiceStaticFactory" factory-method="getBookServiceImpl"></bean>


<!--    实例工厂创建bean     //创建一个对象,通过这个对象里的方法创建对象-->
    <bean id="factory" class="example.factory.factory" ></bean>
    <bean id="bookservice2" factory-bean="factory" factory-method="getbookserviceimpl"></bean>


<!--FactoryBean创建bean-->
    <bean id="bookservice3" class="example.factory.ServiceFactoryBean"></bean>
```





FactoryBean 创建bean

```java
public class ServiceFactoryBean implements FactoryBean<BookServiceImpl> {


    @Override
    public BookServiceImpl getObject() throws Exception {
        return new BookServiceImpl();
    }

    @Override
    public Class<?> getObjectType() {
        return BookServiceImpl.class;
    }

    @Override
    public boolean isSingleton() {
        return false;  //false 创建的都是同一个对象,true 为不同对象

    }
}

```





加入初始操作和销毁操作,

因为applicationContext中没有close方法,而ClassPathXmlApplicationContext有,所以要执行关闭操作时要改成创建ClassPathXmlApplicationContext对象

```java
   <bean id="bookservice" name="service" class="example.service.impl.BookServiceImpl" scope="singleton" init-method="init" destroy-method="destory">
<!--     name 和id一样,是id的别名,scope里的singleton是控制创建对象时同个对象, protptype创建时是不同对象  -->

    </bean>
```

```java
   public static void main(String[] args){

ClassPathXmlApplicationContext ctx=new ClassPathXmlApplicationContext("applicationContext.xml");
BookServiceImpl bookServiceimpl =(BookServiceImpl) ctx.getBean("bookservice");
bookServiceimpl.save();
ctx.close();
    }
```

