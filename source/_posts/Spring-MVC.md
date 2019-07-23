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

## 1. 使用Session

当需要向Session中存入数据时，可以使用`ModelMap`对象将数据进行封装，操作方式与封装转发的数据完全相同，例如：
```java
	modelMap.addAttribute("username", username);
```

需要在当前控制器类之前添加`@SessionAttributes`注解，并且，在注解中显式的指定`ModelMap`中封装的哪些数据是需要存储在Session中的，例如：
```java
    @Controller 
    @RequestMapping(value="user")
    @SessionAttributes("username")
    public class UserController {
        // ...
    }
```

当添加了以上注解后，如果`ModelMap`中被存入了名为`username`的数据，该数据就在Session中，而`ModelMap`中的其它数据依然只能用于转发，也就是数据的作用域只在Request级别。

**关于`@SessionAttributes`注解，其属性的配置可以参考该注解的源代码：**
```java
    @AliasFor("names")
    String[] value() default {};

    @AliasFor("value")
    String[] names() default {};

    Class<?>[] types() default {};
```
>`value`和`names`属性的作用是完全相同，用于指定`ModelMap`中的哪些名称对应的数据需要存放到Session中，可以使用字符串数组表示多个属性，另外，还可以配置`types`属性用于指定Session的数据的数据类型，也可以是数组类型，与配置的`names`保持一致即可。

使用这种做法操作Session非常简单，但是，也存在一系列的问题：

1. 默认情况下，重定向时会把Session中的数据暴露在URL中；
2. 通过`ModelMap`存放的数据一定会在Request的作用域中，所以，通过这种方式存放到Session中的数据，其实在Request中也是存在的；
3. 通过这种方式存放到Session中的数据，不可以通过Session对象的`invalidate()`方法清除！只能通过`SessionStatus`类的`setComplete()`方法进行清除！

更加简单的操作Session的方式就是直接在处理请求的方法中添加`HttpSession`类型的参数，然后在方法体中直接操作即可，例如：
```java
    @RequestMapping("handle_login.do") 
    public String handleLogin(String username, String password,
            ModelMap modelMap, HttpSession session) {
        // 日志
        System.out.println("UserController.handleLogin()");
        System.out.println("\tusername=" + username);
        System.out.println("\tpassword=" + password);

        // 判断用户名是否正确
        if ("root".equals(username)) {
            // 是：判断密码是否正确
            if ("1234".equals(password)) {
                // 是：登录成功，将用户名存入到Session
                // modelMap.addAttribute("username", username);
                session.setAttribute("username", username);
                // 重定向到主页
                return "redirect:../index.do";
            } else {
                // 否：密码错误
                modelMap.addAttribute("errorMessage", "ModelMap：密码错误");
                return "error";
            }
        } else {
            // 否：用户名错误
            modelMap.addAttribute("errorMessage", "ModelMap：用户名错误");
            return "error";
        }
    }
```
>使用这种做法并不存在以上使用`@SessionAttributes`时的各种问题，操作也非常简单，缺点就是不易于执行单元测试！

>可以忽略“不易于执行单元测试”，甚至“不使用`@SessionAttributes`”的原因可能是：可以使用专门的测试工具去测试控制器，所以，在控制器中的方法本身是不需要执行单元测试的，甚至在大型项目中根本就不会使用Session，那各种使用方式都是不需要的！

## 2. SpringMVC的拦截器(Interceptor)

- 如果项目中有多个请求需要执行相同的数据处理方案，就可以使用拦截器来实现。
- 拦截器的作用并不一定是要把请求“拦截下来，不允许向后执行”，其主要特征是：若干种不同的请求都需要先执行拦截器中的代码，才可以向后执行。

当然，拦截器也确实具备“拦截”的功能，即：可以将请求拦截下来，不允许向后执行。

假设需要定义一个“登录拦截器”，实现“如果用户已经登录，则放行，如果未登录，则拦截，不允许向后执行”。

1. 需要自定义`cn.tedu.spring.LoginInterceptor`拦截器类，实现`HandlerInterceptor` (低版本可以通过报错实现方法，高版本不会报错，需要手动实现方法)

2. 拦截器需要在Spring的配置文件中进行配置才可以使用：
```xml
    <!-- 配置拦截器链 -->
    <mvc:interceptors>
        <!-- 配置第1个拦截器 -->
        <mvc:interceptor>
            <!-- 拦截的路径 -->
            <mvc:mapping path="/index.do"/>
            <!-- 拦截器类 -->
            <bean class="cn.tedu.spring.LoginInterceptor" />
        </mvc:interceptor>
    </mvc:interceptors>
```

3. 如果需要实现“验证登录以决定是否拦截或者放行”的功能，需要重写拦截器类中的`preHandle()`方法：
```java
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
            throws Exception {
        System.out.println("LoginInterceptor.preHandle()");
        // 获取HttpSession对象
        HttpSession session = request.getSession();
        // 判断Session中有没有登录的数据
        if (session.getAttribute("username") == null) {
            // 没有登录数据，即未登录，则重定向到登录页
            // http://localhost:8080/XX/index.do
            // http://localhost:8080/XX/user/password.do
            String projectName = request.getServletContext().getContextPath();
            response.sendRedirect(projectName + "/user/login.do");
            // 返回false表示拦截，不允许向后执行
            return false;
        }
        // 返回true表示放行，允许向后执行
        return true;
    }
```
>注意：即使调用了`response.sendRedirect()`进行重定向，如果要阻止继续向后运行，仍然需要`return false;`。

- 在配置拦截器时，使用的是`<mvc:mapping />`配置需要拦截的路径，每个拦截器都可以配置1~N个该节点。
- 在配置路径时，还可以使用`*`作为通配符，例如配置为`<mvc:mapping path="/product/*" />`，则表示例如`/product/index.do`、`/product/add.do`、`/product/list.do`等路径都会被拦截！但是，1个`*`只能通配1层资源，例如`/product/*`就不会匹配上`/product/x/y.do`，如果需要通配若干层路径，可以使用2个`*`，即配置为`/product/**`，使用2个`*`是无视层级的，无论是`/product/list.do`，还是`/product/x/y.do`，甚至更多层级的，都可以通配！
- 另外，还可以添加`<mvc:exclude-mapping />`用于配置例外路径，也就是“白名单”，被添加在白名单中的路径将不被拦截器处理，与`<mvc:mapping />`的配置方式完全相同，可以有多个配置白名单的节点，在配置时，路径中也可以使用通配符。
- 在配置整个`<mvc:interceptor>`节点时，其子级的节点必须先配置`<mvc:mapping />`，再配置`<mvc:exclude-mapping />`，最后配置`<bean>`，不可以颠倒顺序！

### 3. 在SpringMVC中统一处理异常

在Java中，异常的继承体系是：
```
Throwable
    Error
        OutOfMemoryError
    Exception
        SQLException
        IOException
            FileNotFoundException
        RuntimeException
            NullPointerException
            ClassCastException
            ArithmeticException
            NumberFormatException
            IndexOutOfBoundsException
                ArrayIndexOutOfBoundsException
                StringIndexOutOfBoundsException
```

在Exception中，RuntimeException及其子孙类异常是比较特殊的异常，完全不受Java处理异常的语法约束，因为这些异常可能出现的频率极高，并且，这些异常是可以通过更加严谨的编程来杜绝异常的发生的！

常见的处理异常的做法是捕获(try...catch)或者声明抛出(throw/throws)，在实际处理时，如果当前类适合处理异常，就应该使用try...catch捕获并处理，如果当前类不适合处理异常，则应该声明抛出，然后续调用这个方法的角色进行处理。

在服务器端的项目中，通常适合处理异常的都是控制器，但是，某些异常可能在多个不同的功能中都会出现，在处理不同的请求时采取相同的代码进行处理，就会导致代码冗余，不便于统一管理，所以，在SpringMVC框架中就提供了统一处理异常的机制。

**可以在控制器类中添加统一处理异常的方法，关于该方法：**

1. 应该使用`public`权限；
2. 返回值的意义与处理请求的方法完全相同；
3. 方法名称可以自定义；
4. 方法中必须包含异常类型的参数，且参数的类型能包括所有可能需要处理的异常，例如可能处理`NullPointerException`，则参数的类型可以是`NullPointerException`或者`RuntimeException`或者`Exception`甚至`Throwable`，如果同时还需要处理`NumberFormatException`，参数类型就不可以是`NullPointerException`，简单来说，写`Throwable`绝对错不了；
5. 与处理请求的方法不同，不可以随心所欲的添加参数，如果需要转发数据，只能添加`HttpServletRequest`参数，不可以使用`ModelMap`；
6. 必须添加`@ExceptionHandler`注解。

所以，处理请求的方法可以是：
```java
    @ExceptionHandler
    public String handleException(Throwable ex, HttpServletRequest request) {
        if (ex instanceof NullPointerException) {
            request.setAttribute("errorMessage", "空指针异常！");
        } else if (ex instanceof ArrayIndexOutOfBoundsException) {
            request.setAttribute("errorMessage", "数据下标越界异常异常！");
        } else {
            request.setAttribute("errorMessage", "未知异常：" + ex.getClass().getName());
        }

        return "error";
    }
```
>一旦添加了该方法，当前类中任何处理请求的方法都不必处理相关异常，等同于这些方法把异常抛出了，将由以上方法进行统一处理！

>需要注意的是：该方法只能处理当前类处理请求时出现的异常，如果其他控制器类的方法抛出了异常，是不会被处理的！可以把该处理异常的方法放在所有控制器类公共的父类中！

`@ExceptionHandler` 该注解可以指定需要被处理的异常的种类！参数可以是数组，即同时指定多种异常都将被该方法进行处理！

### 拦截器(Interceptor)和过滤器(Filter)的区别

拦截器和过滤器都是可以设置在若干种不同的请求处理之前的，都可以实现“拦截”和“放行”的做法，项目中，都可以存在若干个拦截器或者过滤器形成拦截器链或者过滤器链。

过滤器是JavaEE中的组件，拦截器是SpringMVC中的组件，只要是使用Java语言做服务器端开发都可以使用过滤器，但是，只有使用了SpringMVC框架才可以使用拦截器，并且，如果使用SpringMVC时，`DispatcherServlet`映射的路径是`*.do`，则只有以`.do`为后缀的请求才可能被拦截器处理，也就是说，只有被`DispatcherServlet`映射到的路径才可能被拦截器处理。

过滤器是执行在所有`Servlet`组件之前的，而拦截器是执行在`DispatcherServlet`之后、且在各Controller控制器之前及之后的组件！

过滤器是需要在**web.xml**中进行配置的，其过滤的路径只能通过`<url-pattern>`节点配置1个路径，配置非常不灵活，拦截器可以通过若干个`<mvc:mapping />`节点配置若干个黑名单，还可以通过若干个`<mvc:exclude-mapping />`节点配置若干个白名单，配置就非常灵活！

虽然拦截器和过滤器可以实现的的功能几乎相同，且拦截器的配置更加灵活，但是，由于执行时间节点的差异，拦截器也并不能完全取代过滤器！

