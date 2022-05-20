# **Spring的两个核心部分：IOC和AOP**

1. IOC：控制反转，把创建对象的过程交给Spring进行管理
2. AOP：面向切面，在不修改源代码的前提下，进行功能增强

### IoC概念和原理

Spring IOC 负责创建对象，管理对象（通过依赖注入（DI），装配对象，配置对象，并且管理这些对象的整个生命周期。

使用IOC目的：为了降低耦合度

### IOC 思想基于 IOC 容器完成，IOC 容器底层就是对象工厂，Spring提供的I0C容器实现的两种方式（两个接口）

（1）BeanFactory接口：IOC容器基本实现是Spring内部接口的使用接口，不提供给开发人员进行使用（加载配置文件时候不会创建对象，在获取对象时才会创建对象。）
（2）ApplicationContext接口：BeanFactory接口的子接口，提供更多更强大的功能，提供给开发人员使用（加载配置文件时候就会把在配置文件对象进行创建）推荐使用这一种，将创建对象等的操作在服务运行前完成。

### IOC操作Bean管理

什么是Bean管理：Bean管理指的是两个操作：Spring创建对象和Spring注入熟悉

Bean管理操作有两种方式：**基于xml配置文件方式实现     基于注解方式实现**

**IOC操作Bean管理（基于xml方式）**

1.基于xml方式创建对象

![image-20220219165607943](C:\Users\听风\AppData\Roaming\Typora\typora-user-images\image-20220219165607943.png)

（1）在spring的配置文件中，使用bean标签，标签里面添加属性，就可以创建对象

（2）在bean标签中有许多属性，下面的是一些常用的属性：

- id属性：唯一标识
- class属性：类全路径（包类路径）

（3）创建对象的时候，默认是执行无参数的构造方法

2.基于xml方式注入属性

（1）**DI：依赖注入，就是注入属性**

​     第一种注入方式：使用set方法进行注入

​     第二种注入方式：使用有参数的构造进行注入

3.第一种注入方式，使用set方法进行注入

（1）创建类，定义属性和对应的set方法

​     ![image-20220219171413503](C:\Users\听风\AppData\Roaming\Typora\typora-user-images\image-20220219171413503.png)

（2）在spring的配置文件中配置对象创建，配置属性注入

![image-20220219172002736](C:\Users\听风\AppData\Roaming\Typora\typora-user-images\image-20220219172002736.png)

4.第二种注入方式：使用有参数的构造进行注入

（1）创建类，定义属性，创建属性对应的有参数的构造方法

（2）在配置文件中进行属性的注入

![image-20220219173318632](C:\Users\听风\AppData\Roaming\Typora\typora-user-images\image-20220219173318632.png)

5.p名称空间注入

（1）使用p名称空间注入，可以用于简化xml配置方式

​          第一步 添加p名称空间在配置文件中

![image-20220219194017908](C:\Users\听风\AppData\Roaming\Typora\typora-user-images\image-20220219194017908.png)

​          第二步进行属性的注入，在bean标签中进行操作

![image-20220219194135752](C:\Users\听风\AppData\Roaming\Typora\typora-user-images\image-20220219194135752.png)

### IOC操作Bean管理（xml注入其它类型属性）

1.字面量

​    （1）null值

![image-20220219194641632](C:\Users\听风\AppData\Roaming\Typora\typora-user-images\image-20220219194641632.png)

​    （2）属性值中包含特殊符号

![image-20220219195208504](C:\Users\听风\AppData\Roaming\Typora\typora-user-images\image-20220219195208504.png)

2.注入属性——外部bean

​    （1）创建两个类service类和dao类

​    （2）在service中调用dao中的方法

​    （3）在spring的配置文件中进行配置

```java
<!--    service和dao对象进行创建-->
    <bean id="userService" class="com.zewen.service.UserService">
<!--        注入userDao对象
            name属性值：类里面的属性名称
            ref属性：创建userdao对象bean标签的id值
-->
        <property name="userDao" ref="userDaoImpl"></property>
    </bean>
    <bean id="userDaoImpl" class="com.zewen.dao.UserDaoImpl"></bean>
```

3.注入属性——内部bean和级联赋值

​      （1）一对多关系：部门和员工     一个部门对应多个员工

​      （2）在实体类中表示一对多的关系

​      （3）在spring的配置文件中进行相关的配置

```java
<!--    内部bean-->
    <bean id="emp" class="com.zewen.bean.Emp">
<!--        设置两个普通的属性-->
        <property name="ename" value="Lucy"></property>
        <property name="gender" value="女"></property>
<!--        设置对象类型属性-->
        <property name="dept">
            <bean id="dept" class="com.zewen.bean.Dept">
                <property name="dname" value="安保部"></property>
            </bean>
        </property>
    </bean>
```

4.注入属性——级联赋值

（1）第一种写法

```java
<!--    级联赋值-->
    <bean id="emp" class="com.zewen.bean.Emp">
<!--        设置两个普通的属性-->
        <property name="ename" value="Lucy"></property>
        <property name="gender" value="女"></property>
<!--        级联赋值-->
        <property name="dept" ref="dept"></property>
    </bean>
    <bean id="dept" class="com.zewen.bean.Dept">
        <property name="dname" value="财务部"></property>
    </bean>
```

(2)第二种写法

```java
<!--    级联赋值-->
    <bean id="emp" class="com.zewen.bean.Emp">
<!--        设置两个普通的属性-->
        <property name="ename" value="Lucy"></property>
        <property name="gender" value="女"></property>
<!--        级联赋值-->
        <property name="dept" ref="dept"></property>
        <property name="dept.dname" value="技术部"></property>
    </bean>
    <bean id="dept" class="com.zewen.bean.Dept">
        <property name="dname" value="财务部"></property>
    </bean>
```

### IOC操作Bean管理（xml注入集合属性1）

1.注入数组类型属性

2.注入List集合类型属性

3.注入Map集合类型属性

​    （1）创建类，定义数组、list、map、set类型属性，生成对应的set方法

​	（2）在spring配置文件中进行配置

```java
<!--            1.数组类型属性注入-->
            <property name="courses">
                <array>
                    <value>java课程</value>
                    <value>数据库课程</value>
                </array>
            </property>
<!--            2.list类型属性注入-->
            <property name="list">
                <list>
                    <value>张三</value>
                    <value>李四</value>
                </list>
            </property>
<!--            3.map类型属性注入-->
            <property name="map">
                <map>
                    <entry key="Java" value="java"></entry>
                    <entry key="PHP" value="php"></entry>
                </map>
            </property>
<!--            4.set类型属性注入-->
            <property name="sets">
                <set>
                    <value>Mysql</value>
                    <value>Redis</value>
                </set>
            </property>
        </bean>
```

4.在集合中设置对象类型值

5.把集合中的部分提取出来作为公共部分

（1）在spring的配置文件中引入名称空间

（2）使用util标签完成list集合注入提取

```java
<!--        提取list集合类型属性注入-->
       <util:list id="booklist">
           <value>1.</value>
           <value>2.</value>
           <value>3.</value>
       </util:list>

<!--        提取list集合类型数据属性注入使用-->
    <bean id="book" class="com.zewen.Book">
        <property name="list" ref="booklist"></property>
    </bean>
```

### IOC操作Bean管理（FactoryBean）

1.Spring中有两种类型的Bean，一种是普通的Bean，另一种是工厂Bean(FactoryBean)

2.普通Bean：在配置文件中定义的Bean类型就是返回类型

3.工厂bean：在配置文件中定义的Bean类型可以和返回类型不一样

​    （1）创建一个类， 将这个类作为工厂Bean，实现接口FactoryBean

​       (2)  实现接口中的方法，在实现的方法中定义返回的Bean类型

```java
//定义返回的Bean
    @Override
    public Course getObject() throws Exception {
        Course course = new Course();
        course.setCname("123");
        return course;
    }

```

###  IOC操作Bean管理（Bean的作用域）   

1.在Spring里面，设置创建的实例是单实例还是多实例

2.在Spring里面，在默认情况下，bean是单实例对象

3.Spring配置文件中，如何设置bean是单实例还是多实例

​    （1）scope属性值

​       第一个值是singleton，默认值，表明是单实例对象

​       第二个值是prototype，表明是多实例对象

​    （2）singleton和prototype的区别

​         singleton是单实例，prototype为多实例

​         设置scope值是singleton时，加载spring配置文件会创建一个单实例对象

​         设置scope值是prototype时，不是在加载spring配置文件时候创建对象，是在调用getbean这一                     方法时创建多实例对象

### IOC操作Bean管理（Bean的生命周期） 

1.生命周期：从对象创建到对象销毁的过程

2.bean的生命周期

  (1)通过构造器创建bean的实例（无参数构造）

  (2)为bean的属性设置值和对其他bean引用（调用set方法）

  (3)调用bean的初始化方法（需要进行配置）

  (4)这时，bean就可以使用了（对象获取到了）

  (5)当容器关闭时，调用bean的销毁方法（需要进行配置销毁的具体方法）

3.bean的后置处理器，bean的生命周期有七步

  (1)通过构造器创建bean的实例（无参数构造）

  (2)为bean的属性设置值和对其他bean引用（调用set方法）

  (3)把bean的实例传递给bean的后置处理器方法

  (4)调用bean的初始化方法（需要进行配置）

  (5)把bean的实例传递给bean的后置处理器方法

  (6)这时，bean就可以使用了（对象获取到了）

  (7)当容器关闭时，调用bean的销毁方法（需要进行配置销毁的具体方法）

###  IOC操作Bean管理（xml自动装配）

1.什么是自动给装配：根据指定装配的规则（属性名称或者属性类型），spring自动将配置的属性值进行注入

2.（1）根据属性名称自动注入

```java
 <bean id="emp" class="com.zewen.autowire.Emp" autowire="byName">
<!--              <property name="dept" ref="dept"></property>-->
       </bean>
       <bean id="dept" class="com.zewen.autowire.Dept"></bean>
```

​     (2)根据属性类型进行自动注入

###  IOC操作Bean管理（引入外部属性文件）

1.直接配置数据库信息

（1）配置德鲁伊连接池

（2）引入德鲁伊连接池依赖

2.引入外部属性文件配置数据库连接池

  （1）创建外部属性文件，写入数据库信息

  （2）把外部的properties属性文件引入spring的配置文件中

```java
 <!--引入外部属性文件-->
       <context:property-placeholder location="classpath:jdbc.properties"/>
       <!--配置连接池-->
       <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
              <property name="driverClassName" value="${prop.driverClass}"></property>
              <property name="url" value="${prop.url}"></property>
              <property name="username" value="${prop.userName}"></property>
              <property name="password" value="${prop.password}"></property>
       </bean>
```

###  IOC操作Bean管理（基于注解方式）

1.什么是注解

（1）注解是代码特殊标记，格式：@注解名称（属性名称=属性值。。）
（2）使用注解，可作用在类， 方法，属性上面
（3）注解目的：简化xml配置

2.Spring针对Bean管理中创建对象提供注解

（1）@Component：容器中普通的注解
（2）@Service ：业务逻辑层以及Service层
（3）@Controller： 外部层
（4）@Repository ：dao层即持久层
   功能是一样的，都可以用来创建bean实例

3.基于注解方式实现对象的创建

（1）引入依赖

（2）开启组件扫描，使用context的名称空间

（3）创建类，再类上面添加注解

```java
 <!--开启组件扫描
        1 如果扫描多个包，多个包使用逗号隔开
        2 扫描包上层目录
        -->
    <context:component-scan base-package="com.zewen"></context:component-scan>


    <!-- 示例一
     use-default-filters="false" 表示现在不使用默认 filter，自己配置 filter
     context:include-filter ，设置扫描哪些内容
     type只扫描这种注解类
     expression表示扫描的为该注解类
    -->
    <context:component-scan base-package="com.zewen" use-default-filters="false">
        <context:include-filter type="annotation"
                                expression="org.springframework.stereotype.Controller"/><!--代表只扫描Controller注解的类-->
    </context:component-scan>

    <!-- 示例二
     use-default-filters="false" 表示现在不使用默认 filter，自己配置 filter
     context:exclude-filter ，设置不扫描哪些内容
    -->
    <context:component-scan base-package="com.zewen">
        <context:exclude-filter type="annotation"
                                expression="org.springframework.stereotype.Controller"/><!--表示Controller注解的类之外一切都进行扫描-->
    </context:component-scan>

```

4.基于注解方式实现属性注入

（1）`@Autowired`：根据属性类型自动装配

（2）`@Qualifier(value=" ")`：根据属性名称自动注入

​      @Qualifier注解的使用，需要和@Autowired一起使用

​      可能当一个接口有多个实现类的时候，使用@Autowired根据类型自动注入无法识别使用哪一个实现类，这时使用@Qualifier(value=" ")根据名称进行注入就可以正确找到实现类

（3）`@Resource`：可根据属性类型或者名称注入 @Resource(name = "")

（4）`@Value`：注入普通类型的注入

```java
    @Value("abc")
    private String name;
```

5.纯注解开发

（1）创建配置类，替代xml配置文件

（2）测试类中的编写

```java
 @Test
    public void test1(){
        //加载配置类
        ApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
        UserService userService = context.getBean("userService",UserService.class);
        System.out.println(userService);
        userService.add();

    }
```

