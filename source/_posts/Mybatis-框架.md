---
title: Mybatis-框架
date: 2019-07-24 18:59:27
tags: github
---

## MyBatis框架

#### 作用：

简化数据库编程，开发者只要指定每项数据操作时的SQL语句及对应的抽象方法即可。

#### 1、创建Spring+MyBatis的项目

创建Maven Project，添加需要的依赖：
```xml
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.1</version>
    </dependency>
```

MyBatis框架是可以独立使用的，但是配置相对繁琐，且没有实际价值，通常都是与Spring结合使用的，甚至结合了SpringMVC，所以，需要添加`mybatis-spring`的依赖：

```xml
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.1</version>
    </dependency>
```

MyBatis的底层是基于jdbc实现的，所以，结合Spring使用后，需要添加`spring-jdbc`依赖，该依赖的代码与`spring-webmvc`几乎一样，只是`artifact id`不同，通常，这2个依赖的版本应该完全相同：

```xml
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>4.3.8.RELEASE</version>
    </dependency>
```

还需要添加`mysql-connector-java`的依赖：

```xml
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.16</version>
    </dependency>
```

另外，还需要`commons-dbcp`数据库连接池的依赖：

```xml
    <dependency>
        <groupId>commons-dbcp</groupId>
        <artifactId>commons-dbcp</artifactId>
        <version>1.4</version>
    </dependency>
```
>最后，还需要添加单元测试`junit`依赖。

#### 2、配置数据库连接

创建**db.properties**文件，以确定数据库连接的相关配置：
```properties
    url=jdbc:mysql://localhost:3306/tedu_ums?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    driver=com.mysql.cj.jdbc.Driver
    username=root
    password=root
    initialSize=2
    maxActive=5
```

在Spring的配置文件中读取`properties`配置信息：
```
    <!-- 读取db.properties -->
    <util:properties id="dbConfig" 
        location="classpath:db.properties" />
```

程序运行时，需要使用的数据源是`BasicDataSource`，框架会通过这个类的对象获取数据库连接对象，然后实现数据访问，所以，就需要为这个类的相关属性注入值，把数据库配置信息确定下来：
```xml
    <!-- 配置数据源 -->
    <bean id="dataSource"
        class="org.apache.commons.dbcp.BasicDataSource">
        <property name="url" 
            value="#{dbConfig.url}" />
        <property name="driverClassName" 
            value="#{dbConfig.driver}" />
        <property name="username" 
            value="#{dbConfig.username}" />
        <property name="password" 
            value="#{dbConfig.password}" />
        <property name="initialSize" 
            value="#{dbConfig.initialSize}" />
        <property name="maxActive" 
            value="#{dbConfig.maxActive}" />
    </bean>
```

检验以上完成的配置是否正确，创建测试类，编写并执行测试方法：
```java
    @Test
    public void getConnection() throws SQLException {
        ClassPathXmlApplicationContext ac
            = new ClassPathXmlApplicationContext("spring.xml");

        BasicDataSource ds = ac.getBean("dataSource", BasicDataSource.class);

        Connection conn = ds.getConnection();
        System.out.println(conn);

        ac.close();
    }
```

#### 3、设计接口和抽象方法

创建`cn.tedu.mybatis.User`类，类的属性与`t_user`表保持一致：
```java
    public class User {

        private Integer id;
        private String username;
        private String password;
        private Integer age;
        private String phone;
        private String email;

        // SET/GET/toString/hashCode/equals/Serializable

    }
```

在MyBatis中，要求抽象方法写在接口中，所以，需要先创建`cn.tedu.mybatis.UserMapper`接口，然后，在接口中添加抽象方法，设计原则：

- 如果要执行的操作是`INSERT`/`UPDATE`/`DELETE`，返回值类型使用`Integer`，表示受影响的行数；
- 方法的名称可以自定义，但是，不允许重载；
- 参数列表根据执行SQL语句时的不确定数据来设计。

对于要执行的数据操作，先完成“增加”操作，则添加关于“增加”用户数据的抽象方法：
```
	Integer insert(User user);
```

然后，需要通过配置，是让MyBatis框架知道接口在哪里，所以，在Spring的配置文件中添加配置：

```xml
    <!-- 配置MapperScannerConfigurer -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 指定MyBatis所需的接口在哪里 -->
        <property name="basePackage" value="cn.tedu.mybatis" />
    </bean>
```

#### 4、 配置SQL语句

下载`mybatis-mapper.zip`文件，在项目的**resources**下创建**mappers文件夹，将得到的xml文件复制到mappers**文件夹中，并重命名为**UserMapper.xml**。

然后在该文件中配置SQL语句：
```xml
<!-- namespace：当前XML文件用于配置哪个接口中抽象方法对应的SQL语句 -->
    <mapper namespace="cn.tedu.mybatis.UserMapper">
        <!-- 使用insert节点配置插入数据的SQL语句 -->
        <!-- id：抽象方法的方法名 -->
        <!-- 在#{}中间的是方法的参数User类中的属性名称 -->
        <insert id="insert">
            INSERT INTO t_user (
                username, password,
                age, phone,
                email
            ) VALUES (
                #{username}, #{password},
                #{age}, #{phone},
                #{email}
            )
        </insert>
    </mapper>
```

最后，需要在Spring的配置文件补充配置，使得MyBatis框架知道这些XML文件在哪里，且执行时使用的数据源是哪一个：
```xml
    <!-- SqlSessionFactoryBean -->
    <bean class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- XML文件在哪里 -->
        <property name="mapperLocations"
            value="classpath:mappers/*.xml" />
        <!-- 使用哪个数据源 -->
        <property name="dataSource"
            ref="dataSource" />
    </bean>
```

完成后，在测试类中编写并执行单元测试：
```java
    @Test
    public void insert() {
        ClassPathXmlApplicationContext ac
            = new ClassPathXmlApplicationContext(
                "spring.xml");

        UserMapper userMapper
            = ac.getBean("userMapper", UserMapper.class);
        System.out.println(userMapper.getClass());

        User user = new User();
        user.setUsername("admin");
        user.setPassword("1234");
        Integer rows = userMapper.insert(user);
        System.out.println("rows=" + rows);

        ac.close();
    }
```

#### 5、实时获取新插入的数据的id

在配置`<insert>`节点时，添加`useGeneratedKeys="true"`和`keyProperty="id"`的配置：
```xml
	<insert id="insert" useGeneratedKeys="true" keyProperty="id">
```

然后，执行数据操作后，方法的参数对象中就会被封装自动编号的id值！

以上配置的2个属性，`useGeneratedKeys`表示“要不要获取自动生成的字段的值，即自动编号的值”，`keyProperty`表示“获取到的自动编号的值封装在参数对象的哪个属性中”，所以，在本例中，指的是`User`类中的`id`属性，并不是`t_user`表中的`id`字段。

> 通常，Property单词表示“属性”，类才有属性，数据表没有属性，Field表示“字段”，仅当描述数据表结构时才称之为字段，Column表示“列”，仅当描述查询结果时才称之为列。
