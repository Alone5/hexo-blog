---
title: Spring-MVC
date: 2019-07-20 15:16:31
tags: github
---
## Spring-MVC

### 1. SpringMVC的作用
SpringMVC主要解决了View-Controller交互的问题。

传统的`Controller`具体表现为一个个的`Servlet`类，在一个普通的项目中，需要实现的功能至少有50个以上，假设是50个，则项目中就需要创建50个`Servlet`类去处理这50个功能对应的请求，在**web.xml**中每个`Servlet`类至少需要8行代码进行配置，如果是一个更加复杂的系统，就会导致：`Servlet`实例过多，类文件太多，配置文件太长，所导致的代码开发难度和维护难度大的问题。另外，还存在`Servlet`或其它Java EE中的API功能较弱的问题。

### 2. SpringMVC中的核心组件

- `DispatcherServlet`：前端控制器，用于接收所有请求，并进行分发；
- `HandlerMapping`：记录了请求路径与实际处理请求的控制器之间的对应关系；
- `Controller`：实际处理请求的控制器；
- `ModelAndView`：控制器的处理结果，其中`Model`表示处理请求时得到的即将响应给客户端数据，`View`表示负责响应时显示的视图的名称；
- `ViewResolver`：根据视图的名称，确定视图组件。

{%asset_img MVC.png 流程图%}

### 3. SpringMVC HelloWorld

#### 3.1. 案例目标

在浏览器中，通过`http://localhost:8080/项目名称/hello.do`进行访问，页面中将显示“Hello, SpringMVC!!!”字样。

#### 3.2. 创建项目

创建**Maven Project**，**Packaging**必须选择`war`。
创建完成后，在**web.xml**；添加**spring-webmvc**依赖；复制Spring的配置文件到项目中；添加Tomcat运行环境。

#### 3.3. 通过DispatcherServlet接收所有请求

SpringMVC中已经定义好了`DispatcherSerlvet`类，如果需要该`Servlet`能够接收并处理请求，首先，就需要在**web.xml**中进行配置：
```xml
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>
```
> 在配置时，如果不明确`DispatcherServlet`的包名，可以随意创建某个Java类，在类中声明该`Servlet`，由Eclipse完成导包，则`import`语句中就有了该类的全名。

至此，`DispatcherServlet`就可以接收所有以`.do`为后缀的请求。

默认情况下，所有的`Servlet`都是第一次接收并处理请求时才完成的初始化操作，并且，SpringMVC框架是基于Spring框架基础之上的，在项目启动初期，就应该加载整个Spring环境，以保证SpringMVC框架能正常运行！所以，可以通过配置去实现：当启动Tomcat时，就直接初始化`DispatcherServlet`，在`DispatcherServlet`中，已经定义了某个属性，值是Spring配置文件的位置，当`DispatcherServlet`被初始化时，就会读取该配置文件！

在配置时，可以通过`<load-on-startup>`使得`DispatcherServlet`是默认直接初始化的！

在`DispatcherServlet`的父类`FrameworkServlet`中，有`contextConfigLocation`属性，其作用就是制定Spring配置文件的位置，一旦创建了`DispatcherServlet`对象，就会自动读取所配置的文件，以加载Spring的环境！则配置为：

```xml
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring.xml</param-value>
    </init-param>
```

`DispatcherServlet`的完整配置：
```xml
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>
```
>以上配置实现的效果就是：当Tomcat启动时，就会创建`DispatcherServlet`的对象，并且，`DispatcherServlet`会读取所配置的`spring.xml`文件，以加载Spring环境，后续，`DispatcherServlet`将接收所有以`.do`为后缀的请求！

验证现在的配置结果，可以自定义`cn.tedu.spring.User`类，然后，在类中显式的定义构造方法：
```java
    public class User {
        public User() {
            System.out.println("创建了User的对象！");
        }
    }
```

在Spring的配置文件中进行配置：
```xml
	<bean class="cn.tedu.spring.User" />
```

由于Tomcat启动时就会初始化`DispatcherServlet`，进而加载Spring配置文件，就会由Spring框架创建`User`对象，导致构造方法被执行，所以，当启动Tomcat时，就会看到构造方法中输出的内容！

#### 3.4. 使用控制器接收请求

首先创建`cn.tedu.spring.HelloController`控制器类，在类之前添加`@Controller`注解：
```java
    package cn.tedu.spring;

    import org.springframework.stereotype.Controller;

    @Controller
    public class HelloController {

    }
```

然后，在Spring的配置文件设置组件扫描：
```xml
    <!-- 组件扫描 -->
    <context:component-scan base-package="cn.tedu.spring" />
```

控制类中需要添加处理请求的方法，方法的设计原则是：
1. 应该使用`public`权限；
2. 暂时使用`String`作为返回值类型；
3. 方法名称自由定义；
4. 方法中暂时不添加参数。

方法之前添加`@RequestMapping`注解，用于配置请求路径，以确定请求路径与处理请求的方法的对应关系：
```java
    @RequestMapping("hello.do")
    public String showHello() {
    	//测试代码
        System.out.println("HelloController.showHello()");
        return null;
    }
```
>打开浏览器，输入`http://localhost:8080/SPRINGMVC-01/hello.do`，在浏览器中会提示404错误，因为目前尚未处理页面，在Eclipse的控制台应该可以看到以上输出的内容！每访问一次，就会出现1次输出语句！

#### 3.5. 响应页面

处理请求的方法的参数是`String`类型时，默认表示响应给客户端的视图的名称，后续，会经过`ViewResolver`进行处理，得到具体的视图。

首先，创建jsp页面，例如在**WEB-INF**下创建**index.jsp**文件，页面内容随意。

然后，设计处理请求的方法的返回值，并配置`ViewResolver`！`ViewResolver`是一个接口，具体使用的实现类是`InternalResourceViewResolver`，需要在Spring的配置文件中配置它的`prefix`和`suffix`属性，分别表示“前缀”和“后缀”，`InternalResourceViewResolver`的工作模式是以**webapp**文件夹为根路径，将“前缀 + 处理请求的方法的返回值 + 后缀”得到具体的JSP文件的位置，刚才创建的JSP文件在**webapp/WEB-INF/index.jsp**，则可以：

```
    /WEB-INF/   +   index   +   .jsp

    ""  +   /WEB-INF/index.jsp  +   ""
```

还可以是其他组合方式，只要能组合出文件的路径都是正确的，可以在处理请求的方法中返回`"index"`，配置为：
```xml
    <!-- 视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/" />
        <property name="suffix" value=".jsp" />
    </bean>
```
>开发完成，再次测试访问。