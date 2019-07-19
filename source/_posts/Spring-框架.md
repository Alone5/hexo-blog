---
title: Spring-框架
date: 2019-07-18 19:07:14
tags: github
---
## Spring
### Spring简介
Spring是一个开源框架，Spring是于2003 年兴起的一个轻量级的Java 开发框架，由Rod Johnson 在其著作Expert One-On-One J2EE Development and Design中阐述的部分理念和原型衍生而来。它是为了解决企业应用开发的复杂性而创建的。框架的主要优势之一就是其分层架构，分层架构允许使用者选择使用哪一个组件，同时为 J2EE 应用程序开发提供集成的框架。Spring使用基本的JavaBean来完成以前只可能由EJB完成的事情。然而，Spring的用途不仅限于服务器端的开发。从简单性、可测试性和松耦合的角度而言，任何Java应用都可以从Spring中受益。

- 通过设计模式中的工厂模式来创建对象：
```java
    public class UserDaoFactory {
        public static IUserDao newInstance() {
            return new UserDao2();
        }
    }
```

- 然后，代码可以进一步调整为：
```java
	public IUserDao userDao = UserDaoFactory.newInstance();
```

- 则后续需要使用`IUserDao`对象时，声明成以上代码即可，如果需要更换具体的实现类对象，也只需要修改工厂类中的返回值，类似以上声明的代码，无论在项目中出现过多少次，都是不需要调整的！
- 所以，总的看来，通过工厂来创建对象，前期编写的代码会更多一些，但是，可以实现“解耦”的效果，后期的管理难度会大大的降低！
- 在实际项目开发中，不可能为所有的类或者大量的类去设计专属工厂，而Spring框架就相当于一个大工厂，可以生产我们配置的、希望它去生产的所有对象！后续，当需要获取对象时，直接通过Spring获取即可，并不需要自己创建对象，所以，Spring框架也被称之为“Spring容器”。

### 1、Spring 示例：
创建`Maven Project`，Packaging选择`war`
创建出来的项目默认没有**web.xml**文件，作为WEB项目来说是错误的，所以，需要先生成该文件。
需要添加所以依赖的框架，在**pom.xml**中添加：
```xml
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>4.3.8.RELEASE</version>
    </dependency>
```
>如果下载的jar包有问题，可以尝试换个版本，目前，对使用的Spring的要求是不低于4.2版本。

当项目创建完成后，下载http://doc.tedu.cn/config/spring-context.zip文件，将压缩包中的文件**applicationContext.xml**复制到项目中的**src/main/resources**中。

假设希望通过Spring创建`java.util.Date`类的对象，则应该在**applicationContext.xml**中添加配置：
```xml
    <!-- id:当从Spring容器中获取该对象时使用的名称 -->
    <!-- class:需要创建的类 -->
    <bean id="date" class="java.util.Date"></bean>
```

通过单元测试查看配置效果，需要先添加单元测试的依赖：
```xml
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
```

然后，创建测试类：
```java
    public class TestCase {
        @Test
        public void getDate() {
            // 1. 加载Spring配置文件，获得Spring容器
            ClassPathXmlApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
            // 2. 从Spring容器中获取对象
            Date date = (Date) ac.getBean("date");
            // 3. 测试所获取的对象
            System.out.println(date);
            // 4. 关闭Spring容器，释放资源
            ac.close();
        }
    }
```

#### 通过Spring创建对象的方式
**(a) 类中存在无参数的构造方法（实用，掌握）**
以`java.util.Date`类为例，当Spring创建对象时，会自动调用其无参数构造方法。
在Spring的配置文件中，配置方式为：
```xml
	<bean id="date" class="java.util.Date"></bean>
```
>如果某个类中并不存在无参数的构造方法，则不可以通过这种方式进行配置。

**(b) 类中存在静态工厂方法（不实用，了解）**
如果某个类中存在某个`static`修饰的方法，且方法的返回值类型就是当前类的类型，例如`java.util.Calendar`类就是如此：
```java
    public abstract class Calendar {
        public static Calendar getInstance() {
            // ...
        }
    }   
```

符合这种条件的类，可以配置为：
```xml
    <!-- (b) 类中存在静态工厂方法 -->
    <!-- factory-method:工厂方法的名称 -->
    <bean id="calendar" class="java.util.Calendar" 
		factory-method="getInstance">
    </bean>
```

**(c) 存在实例工厂方法（更不实用，了解）**
在其他类中，存在某个工厂方法，可以创建指定的类的对象，例如：
```java
    public class Phone {
        public Phone(String name) {
        }
    }

    public class PhoneFactory {
        public Phone newInstance() {
            return new Phone("HuaWei");
        }
    }
```

当需要Spring创建`Phone`对象时，可以让Spring先创建`PhoneFactory`的对象(实例)，然后调用该对象的`newInstance()`方法，即可获取`Phone`的对象，从而完成对象的创建过程。

具体配置为：
```xml
    <!-- (c) 存在实例工厂方法 -->
    <!-- factory-bean:配置为工厂类的bean-id -->
    <bean id="phoneFactory" class="cn.tedu.spring.PhoneFactory">
    </bean>
    <bean id="phone"
        class="cn.tedu.spring.Phone"
        factory-bean="phoneFactory"
        factory-method="newInstance">
    </bean>
```

### 由Spring管理的对象的作用域与生命周期

**(a) 由Spring管理的对象的作用域**
单例：单一实例，在某个时刻，可以获取到的同一个类的对象将有且仅有1个，如果反复获取，并不会得到多个实例。
单例是一种设计模式，其代码格式可以是：
```java
    public class King {
        private static King king = new King();

        private King() {
        }

        public static King getInstance() {
            return king;
        }
    }
```

以上单例模式也称之为“饿汉式单例模式”，其工作特性为“程序运行初期就已经创建了该类的实例，随时获取实例时，都可以直接获取”，还有另外一种单例模式是“懒汉式单例模式”，其工作特性为“不到逼不得已不会创建类的对象”，其代码是：
```java
    public class King {
        private static final Object LOCK = new Object();
        private static King king;
        private King() {
        }
        public static King getInstance() {
            if (king == null) {
                synchronized (LOCK) {
                    if (king == null) {
                        king = new King();
                    }
                }
            }
            return king;
        }
    }
```

- 理论上来说，懒汉式的单例模式可能可以节省一部分性能消耗，例如整个系统运行了30分钟之后才获取对象的话，则前30分钟是不需要创建对象的！但是，这种优势只是理论上的优势，在实际应用时，2种单例模式的差异表现的并不明显！
- 通过以上单例模式的设计方式，可以发现单例的对象都是使用了`static`修饰的！所以，具有“唯一、常驻”的特性！

在Spring管理对象时，可以配置`lazy-init`属性，以配置是否为懒汉式的单例模式：
```xml
    <!-- lazy-init：是否懒加载，取值为true或false(默认) -->
    <bean id="user" 
        class="cn.tedu.spring.User"
        lazy-init="true">
    </bean>
```

在Spring管理对象时，可以配置`scope`属性，以配置类的对象是否是单例的：
```xml
    <!-- scope:是否单例，取值为singleton(默认，单例)或prototype(非单例) -->
    <bean id="user" 
        class="cn.tedu.spring.User"
        scope="prototype">
    </bean>
```

**(b) 由Spring管理的对象的生命周期**

- 生命周期描述了某个对象从创建到销毁的整个历程，在这个历程中，会被调用某些方法，这些方法就是生命周期方法，学习生命周期的意义在于了解这些方法的调用特征，例如 何时调用、调用次数，以至于开发功能时，可以将正确的代码重写在正确的方法中！
- 以Servlet为例，其生命周期方法主要有`init()`、`service()`、`destroy()`，其中，`init()`和`destroy()`都是只执行1次的方法，而`service()`在每次接收到请求时都会被调用，且`init()`方法是创建对象之后就会执行的方法，`destroy()`是销毁对象的前一刻执行的方法，所以，如果有某个流对象需要初始化，初始化的代码写在`destroy()`方法中就是不合适的！反之，如果要销毁流对象，也不能把代码写在`init()`中！

由Spring管理的单例对象也并不是开发人员完全可控的，即不知道何时创建的，何时销毁的，所以，Spring提供了“允许自定义初始化方法和销毁方法”的做法，开发人员可以自行定义2个方法分别表示初始化方法和销毁方法：
```java
    public class User {
        public User() {
            super();
            System.out.println("创建User对象……");
        }

        public void init() {
            System.out.println("初始化方法被调用……");
        }

        public void destroy() {
            System.out.println("销毁方法被调用……");
        }
    }
```

配置：
```xml
    <!-- init-method：初始化方法的名称 -->
    <!-- destroy-method：销毁方法的名称 -->
    <bean id="user" 
        class="cn.tedu.spring.User"
        init-method="init"
        destroy-method="destroy">
    </bean>
```
>注意：生命周期方法的配置是建立在“单例”基础之上的，如果对象并不是单例的，讨论生命周期就没有意义了。

### 2、Spring IOC

#### 1、通过SET方式注入属性的值
假设存在：
```java
    public class UserDao {
        //如果需要为`driver`注入值，首先需要为该属性添加SET方法
        public String driver; // com.mysql.jdbc.Driver

        public void setDriver(String driver) {
            this.driver = driver;
        }
    }
```

Spring的配置文件中，添加`<property>`节点以进行配置：
```xml
    <bean id="userDao" class="cn.tedu.spring.UserDao">
        <property name="driver" value="com.mysql.jdbc.Driver" />
    </bean>
```

另外，如果某个属性的值并不是基本值（在Spring中，把基本数据类型的值和字符串统一称为基本值），例如：
```java
public class UserServlet {
    public UserDao userDao;
}
```

以上属性的值应该是前序创建的`UserDao`类的对象，则，在配置时，可以通过`<property>`节点的`ref`属性引用那个`<bean>`的`id`：
```xml
    <bean id="userServlet" class="cn.tedu.spring.UserServlet">
        <property name="userDao" ref="userDao" />
    </bean>
```

- 综合来看，无论属性的值是什么类型，只要是通过SET方式注入属性值，首先都必须为属性添加SET方法，然后在`<bean>`节点下级通过`<property>`节点进行配置，如果属性的值是基本值，则使用`value`属性直接编写对应的值，如果属性的值不是基本值，则使用`ref`属性引用另一个`<bean>`的`id`（如果没有所说的另一个`<bean>`，就想办法配出这样一个`<bean>`）。

#### 2、通过构造方法注入属性的值
假设存在：
```java
public class AdminDao {
    public String url;

    public AdminDao(String url) {
        super();
        this.url = url;
    }
}
```

Spring配置文件中：
```xml
    <!-- 通过构造方法注入属性值 -->
    <bean id="adminDao" class="cn.tedu.spring.AdminDao">
        <!-- index：参数的序号，即第几个参数 -->
        <constructor-arg index="0" value="jdbc:mysql://..." />
    </bean>
```

#### 小结
- 通过SET方式注入必须为属性添加规范的SET方法，在配置时，使用`<property>`节点注入属性的值，该节点中，`name`属性可以理解为就是属性名称；
- 通过构造方法注入必须自定义带参数的构造方法，且构造方法会基于参数为属性赋值；
- 无论通过哪种方式，如果注入的值是基本值，通过`value`属性配置，如果注入的值是引用其他Bean，通过`ref`引用那个`<bean>`的`id`。
- 通常推荐为绝大部分类提供无参数的构造方法，所以，通过SET方式注入是比较实用的做法，而通过构造方法注入的使用频率就非常低。

#### 3、注入集合类型的属性值
**1. List**
假设存在：
```java
    // Frank, Andy, Lucy, Kate
    //通过SET方式注入属性的值，需要先 生成SET方法
    public List<String> names;
```

配置为：
```xml
    <!-- 注入List类型的值：Frank, Andy, Lucy, Kate -->
    <property name="names">
        <list>
            <value>Frank</value>
            <value>Andy</value>
            <value>Lucy</value>
            <value>Kate</value>
        </list>
    </property>
```
>Spring框架在处理时，会使用`ArrayList`封装`List`类型的属性的值。

**2. Set**
假设存在：
```java
    // Beijing, Shanghai, Guangzhou, Shenzhen
    public Set<String> cities;
```

配置为：
```xml
    <!-- 注入Set类型的值：Beijing, Shanghai, Guangzhou, Shenzhen -->
    <property name="cities">
        <set>
            <value>Beijing</value>
            <value>Shanghai</value>
            <value>Guangzhou</value>
            <value>Shenzhen</value>
        </set>
    </property>
```
>Spring框架在处理时，会使用`LinkedHashSet`封装`Set`类型的属性的值。

**3. 数组**
假设存在：
```java
    // { 9, 5, 2, 7 } 
    public int[] numbers;
```

配置为：
```xml
    <!-- 注入数组类型的值：{ 9, 5, 2, 7 } -->
    <property name="numbers">
        <array>
            <value>9</value>
            <value>5</value>
            <value>2</value>
            <value>7</value>
        </array>
    </property>
```
>在Spring框架中，注入`List`集合类型的值和数组类型的值时，使用`<list>`节点或者`<array>`节点都是可以的，即：数据是`List`类型的，使用`<array>`来配置，或者数据是数组类型的，使用`<list>`来配置，都是正确的。

**4. Map**
假设存在：
```java
    // username=root, password=1234, from=Hangzhou, age=26
    public Map<String, String> session;
```

配置为：
```xml
    <!-- 注入Map类型的值：username=root, password=1234, from=Hangzhou, age=26 -->
    <property name="session">
        <map>
            <entry key="username" value="root" />
            <entry key="password" value="1234" />
            <entry key="from" value="Hangzhou" />
            <entry key="age" value="26" />
        </map>
    </property>
```

**5. Properties**
在配置`Properties`类型的属性值时，其配置的节点结构是：
```xml
    <!-- 注入Properties类型的值 -->
    <property name="config">
        <props>
            <prop key="username">root</prop>
            <prop key="password">1234</prop>
        </props>
    </property>
```

也可以准备**.properties**文件。**创建db.properties**文件：
```properties
url=jdbc:mysql://localhost:3306/db_name?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
driver=com.mysql.jdbc.Driver
username=root
password=1234
```

在Spring的配置文件中，使用`<util:properties>`节点就可以直接读取**.properties**文件：
```xml
    <util:properties id="config" 
        location="classpath:db.properties" />
```

以上`<util:properties>`节点也是一种`<bean>`节点，所以，注入值时，可以通过`ref`引用这个节点：
```xml
    <!-- 注入Properties类型的值 -->
    <property name="config" ref="config" />
```

### Spring表达式
Spring表达式是使用占位符的方式定义在Spring的配置文件中的，作用是用于获取其他`<bean>`中的属性的值！

假设存在：
```java
public class ValueBean {
    // 值是SampleBean中的names中的第2个
    public String name;
	//通过SET方式注入，也可以通过构造方法注入
	public void setName(String name) {
    this.name = name;
	}
}
```

在Spring的配置文件中进行配置：
```xml
    <!-- 使用Spring表达式 -->
    <bean id="valueBean" class="cn.tedu.spring.ValueBean">
        <!-- 值是SampleBean中的names中的第2个 -->
        <property name="name"
            value="#{sampleBean.names[1]}" />
    </bean>
```

1. Spring表达式的基本格式是使用`#{}`进行占位，其内部语法是：
```
    #{bean的id.属性}
```

2. 如果属性的类型是`List`集合、`Set`集合或者数组，则在属性右侧使用`[]`写出索引或者下标：
```
	#{bean的id.属性[索引或者下标]}
```

3. 如果属性的类型是`Map`集合或者`Properties`，可以使用的语法：
```
    1. #{bean的id.属性.key}
    2. #{bean的id.属性['key']}
```

### 自动装配(autowire)
自动装配：不需要在Spring的配置文件中进行属性值的注入，只需要配置允许自动装配，Spring就会自动的完成属性值的注入。

假设存在`StudentServlet`依赖于`StudentDao`：
```java
public class StudentServlet {
    public StudentDao studentDao;

    public void setStudentDao(StudentDao studentDao) {
        this.studentDao = studentDao;
    }

    public void reg() {
        System.out.println("StudentServlet.reg()");
        studentDao.reg();
    }
}

public class StudentDao {

    public void reg() {
        System.out.println("StudentDao.reg()");
    }
}
```

配置为：

```xml
    <bean id="studentDao"
        class="cn.tedu.spring.StudentDao" />
    <bean id="studentServlet" 
        class="cn.tedu.spring.StudentServlet"
        autowire="byName">
    </bean>
```
>以上配置中，`autowire`属性就是用于配置自动装配的。

- 当取值是`byName`时，表示“按照名称自动装配”，在这个过程中，Spring会先检测到在`StudentServlet`中有名为`studentDao`的属性，会根据该属性得到`setStudentDao`这个方法名称，然后，尝试找到与SET方法名称对应的bean的id，即查找某个id为`studentDao`的`<bean>`，如果找到，则自动调用`setStudentDao`方法来完成自动装配，如果没有找到匹配的bean-id，则不会尝试自动装配。简单的来说，就是**SET方法的名称需要与bean-id相对应**，属性的名称可以和bean-id不对应。自动装配是一种“尝试性”的操作，能装就装，装不上也不会报错！
- 还可以取值为`byType`，表示“按照类型自动装配”，在这个过程，Spring会检测`StudentServlet`中以`set`作为前缀的方法，并尝试调用这些方法，调用时，**会在Spring容器中查找与参数类型相符合的bean，如果没有匹配的对象，则不自动装配，如果找到1个，则执行该方法，以完成自动装配，如果找到2个或者更多，则直接报错错误**。

### Spring注解

#### 1、 通用注解
使用注解的方式来管理对象时，就不必在Spring的配置文件中使用`<bean>`节点进行配置了，但是，需要先配置一项“组件扫描”，使得Spring框架知道需要管理的类在哪里：
```xml
    <!-- 配置组件扫描的根包 -->
    <context:component-scan base-package="cn.tedu.spring" />
```

为需要创建对象的类添加`@Component`注解即可：
```java
    @Component
    public class UserDao {
    }
```
>“组件扫描 + 注解”就可以实现Spring创建对象！

在配置组件扫描时，`base-package`表示“根包”，假设类都在`cn.tedu.spring`包中，可以直接配置为这个包。例如实际使用的是`cn.tedu.spring.dao`、`cn.tedu.spring.servlet`等，可以把根包设置为`cn.tedu.spring`。！

关于使用的注解，可以是：
- `@Component`：通用注解
- `@Controller`：添加在控制器之前的注解
- `@Service`：添加在业务类之前的注解
- `@Repository`：添加在处理持久层的类之前的注解
>在配置Spring创建和管理对象时，在类之前添加以上4个注解中的任意1个即可，以上4个注解的作用相同，使用方式相同，语义不同。

在使用以上注解后，由Spring创建的对象的bean-id默认就是类名首字母改为小写的名称，例如`UserDao`类的默认bean-id就是`userDao`，如果需要自定义bean-id，可以对注解进行配置，例如：
```java
    @Component("dao")
    public class UserDao {
    }
```

#### 2、关于作用域与生命周期的注解
- 使用`@Scope`注解可以配置某类的对象是否是单例的，可以在该注解中配置属性为`singleton`或`prototype`，当配置为`@Scope("prototype")`时表示非单例的，如果希望是单例，则不需要配置该注解，默认就是单例的。
- 在单例的前提下，默认是饿汉式的单例，如果希望是懒汉式的单例模式，可以在类之前添加`@Lazy`注解，在该注解中还可以配置`true`或`false`，例如`@Lazy(false)`，但是，没有必要这样配置，如果希望是饿汉式的，根本就不需要添加该注解，如果希望是懒汉式的，只需要配置`@Lazy`即可，而不需要写成`@Lazy(true)`。

在被Spring管理的类中，可以自定义方法，作为初始化方法和销毁时调用的方法，需要添加`@PostConstruct`和`@PreDestroy`注解，例如：
```java
    @Component
    public class UserDao {

        public UserDao() {
            System.out.println("创建UserDao的对象！");
        }

        @PostConstruct
        public void init() {
            System.out.println("UserDao.init()");
        }

        @PreDestroy
        public void destroy() {
            System.out.println("UserDao.destroy()");
        }
        
    }
```
>以上2个注解是`javax`包中的注解，使用时，需要为项目添加Tomcat运行环境，以使用Java EE相关的jar包，才可以识别以上2个注解！

#### 3.、自动装配
假设存在：
```java
    @Repository
    public class UserDao {

        public void reg() {
            System.out.println("UserDao.reg()");
        }

    }
```

如果存在`UserServlet`需要依赖于以上`UserDao`，则在`UserServlet`中的属性之前添加`@Autowired`注解即可，例如：
```java
    @Controller
    public class UserServlet {

        @Autowired
        public UserDao userDao;

        public void reg() {
            System.out.println("UserServlet.reg()");
            userDao.reg();
        }

    }
```

当然，以上2个类都必须是被Spring所管理的，即：都在组件扫描的包下，且都添加了`@Component`或等同功能的注解。

通过注解实现自动装配时，并不需要属性有SET方法！Spring框架就是将值直接赋值过去的！

使用`@Resource`注解也可以实现自动装配，它是Java EE中的注解，它的装配模式是：优先`byName`实现装配，如果装配失败，会尝试`byType`实现装配，且，如果`byType`装配，要求匹配类型的对象必须有且仅有1个，无论是0个还是超过1个，都会报告错误！

使用`@Resource`时还可以配置需要注入的bean的id，例如：
```
	@Resource(name="ud1")
```

使用`@Autowired`时，会优先`byType`，如果找到1个匹配类型的对象，则直接装配，如果没有匹配类型的对象，则直接报告错误，如果找到多个匹配类型的对象，则会尝试`byName`，如果`byName`装配失败，则报告错误！