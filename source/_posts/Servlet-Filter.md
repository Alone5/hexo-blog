---
title: Servlet-Filter
date: 2019-07-15 19:52:44
tags: github
---
## 过滤器
- JSP 和 Servlet 中的过滤器都是 Java 类。
- 过滤器可以动态地拦截请求和响应，以变换或使用包含在请求或响应中的信息。
- 可以将一个或多个过滤器附加到一个 Servlet 或一组 Servlet。过滤器也可以附加到 JSP 文件和 HTML 页面。
- 过滤器是可用于 Servlet 编程的 Java 类，可以实现以下目的：
	- 在客户端的请求访问后端资源之前，拦截这些请求。
	- 在服务器的响应发送回客户端之前，处理这些响应。
- 过滤器通过 Web 部署描述符（web.xml）中的 XML 标签来声明，然后映射到您的应用程序的部署描述符中的 Servlet 名称或 URL 模式。
- 当 Web 容器启动 Web 应用程序时，它会为您在部署描述符中声明的每一个过滤器创建一个实例。
- Filter 的执行顺序与在 web.xml 配置文件中的配置顺序一致，一般把 Filter 配置在所有的 Servlet 之前。
{% asset_img Filter.png 过滤过程(图片转自菜鸟教程) %}

### 1、过滤器的一个实例
- 利用Filter可以实现如：权限检查、编码过滤、请求日志等功能
1. 编写Servlet
```java
public class TargetServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;
    /**
     * 被过滤器调用的目标 (Target)Servlet
     * 请求URL /target
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("Target");
        response.setContentType("text/html;charset=UTF-8");
        response.getWriter().println("OK");
    }
}
```
2. 编写 Filter
```java
public class DemoFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)throws IOException, ServletException {
        //chain 链节、链
        System.out.println("Hello");
        //执行后续的链节，就是执行后续的 Servlet
        chain.doFilter(request, response); 
        System.out.println("World!");
    }
    @Override
    public void destroy() {
    }
    @Override
    public void init(FilterConfig arg0) throws ServletException {
    }
}
```
3. 配置：
```xml
    <servlet>
        <description></description>
        <display-name>TargetServlet</display-name>
        <servlet-name>TargetServlet</servlet-name>
        <servlet-class>day10.TargetServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>TargetServlet</servlet-name>
        <url-pattern>/target</url-pattern>
    </servlet-mapping>
    <!-- 配置过滤器 -->
    <filter>
        <filter-name>demo</filter-name>
        <filter-class>day10.DemoFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>demo</filter-name>
        <url-pattern>/target</url-pattern>
    </filter-mapping>
```
4. 测试：
```
http://localhost:8080/Servlet10/target
```

### 2、过滤器的生命周期
过滤器的生命周期分为四个阶段：实例化、初始化、过滤和销毁：实例化是指在Web工程的web.xml文件里声明一个过滤器，在声明了过滤器之后，Web容器会创建一个过滤器的实例。
- Servlet对象从创建到使用，以及最后销毁的过程 实例：
1. 创建Servlet
```java
public class InitRootServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    @Override
    public void init() throws ServletException {
    	ServletContext ctx=getServletContext();
        String contextPath = ctx.getContextPath();
        //将ContextPath存储到ServletContext中
        ctx.setAttribute("root", contextPath);
        System.out.println("初始化了");
    }
    @Override
    public void destroy() {
        System.out.println("销毁！");
    }
    /**
     * 在 ServletContext中初始化 root 变量
     * 请求URL： /init-root
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=UTF-8"); 
        response.getWriter().print("OK"); 
    }
}
```
2. 配置：
```xml
    <servlet>
        <description></description>
        <display-name>InitRootServlet</display-name>
        <servlet-name>InitRootServlet</servlet-name>
        <servlet-class>day11.InitRootServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>InitRootServlet</servlet-name>
        <url-pattern>/init-root</url-pattern>
    </servlet-mapping>
```
>注意： 设置<load-on-startup>1</load-on-startup>可以在Web容器启动时候执行Servlet init()方法。 实现容器启动时候初始化ContextPath。

3. 删除所有Servlet中初始化 ContextPath 的代码。
4. 启动应用测试软件功能，在控制台观察打桩输出结果。
- JSP 也是Servlet，其生命周期类似
	1. JSP在第一次请求时候先翻译为Servlet 源文件
	2. 然后编译为.class
	3. 与Servlet生命周期相同

### 3、过滤器的常用方法
- 过滤器最常用的方法有三个：init()、doFilter()和destory()。
1. init()方法：这是过滤器的初始化方法，在Web容器创建了过滤器实例之后将调用这个方法进行一些初始化的操作，这个方法可以读取web.xml中为过滤器定义的一些初始化参数。
2. doFilter()方法：这是过滤器的核心方法，会执行实际的过滤操作，当用户访问与过滤器关联的URL时，Web容器会先调用过滤器的doFilter方法进行过滤。
3. destory()方法：这是Web容器在销毁过滤器实例前调用的方法，主要用来释放过滤器的资源等。

## 过滤器解决授权问题（权限检查）
目标： 对于  / emp / * 等路径的请求进行拦截，如果登录了才能使用。
Servlet、Filter的路径匹配规则：
1. 精确匹配：精确匹配一个具体的URL
   1. /emp/list
   2. /emp/add
2. 路径匹配：匹配路径下一系列URL
   1. / emp / * 
   2. / user / * 
   3. / dept / * 
3. 后缀匹配或扩展名匹配: 满足后缀条件的被匹配上
   1. * .html
   2. * .do
   3. * .png
4. 任意匹配: 全部都匹配
   1. /
   2. / * 

案例步骤：
1. 添加过滤器检查权限
```java
   /**
    * 访问控制过滤器：只有登录的用户才能通过访问
    */
   public class AccessFilter implements Filter {
       @Override
       public void doFilter(ServletRequest request,ServletResponse response, FilterChain chain)throws IOException, ServletException {
           //转换request和response对象的类型，这样可以
           //使用更多的API方法
           HttpServletRequest req=(HttpServletRequest)request;
           HttpServletResponse res=(HttpServletResponse)response;
           HttpSession session = req.getSession();
           User loginUser = (User)session.getAttribute("loginUser");
           if(loginUser==null) {
               System.out.println("没有登录，转到登录"); 
               //没有登录，重定向到登录页面，不执行后续链节
               String login=req.getContextPath()+"/user/start-login";
               res.sendRedirect(login);
               return; //不执行后续链节
           }
           //执行后续链节，就是执行后续的Servlet
           System.out.println("登录成功，继续执行"); 
           chain.doFilter(req, res); 
       }
       @Override
       public void init(FilterConfig arg0) throws ServletException {
       }
       @Override
       public void destroy() {
       }
   }
```

2. 配置：
```xml
   <filter>
       <filter-name>acl</filter-name>
       <filter-class>day11.AccessFilter</filter-class>
   </filter>
   <filter-mapping>
       <filter-name>acl</filter-name>
       <url-pattern>/emp/*</url-pattern>
   </filter-mapping>
   <filter-mapping>
       <filter-name>acl</filter-name>
       <url-pattern>/dept/*</url-pattern>
   </filter-mapping>
   <filter-mapping>
       <filter-name>acl</filter-name>
       <url-pattern>/photo/*</url-pattern>
   </filter-mapping>
```

3. 测试

**过滤器可以叠加使用，方便灵活的重构**
```xml
    <filter>
        <filter-name>demo</filter-name>
        <filter-class>day10.DemoFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>demo</filter-name>
        <url-pattern>/target</url-pattern>
    </filter-mapping>
    <filter-mapping>
        <filter-name>demo</filter-name>
        <url-pattern>/emp/*</url-pattern>
    </filter-mapping>
```

### ServletContext/Application
- ServletContext： Servlet上下文，也就是当前Servlet的运行环境,就代表当前的容器对象，ServletContext API方法提供了与当前容器有关的方法。
- SertvletContext提供了数据共享功能，其共享范围是全部的Servlet和JSP

案例 获取当前服务器的版本信息：
```java
public class ContextDemoServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;
    /**
     * 演示 ServletContext 的功能
     * 请求URL /context-demo
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    /*
     * 1. 获得ServletContext: getServletContext()方法是在HttpServlet类上定义
     * 	调用这个方法可以获得ServletContext对象， ServletContext对象是单例对象
     * 	任何方法得到的ServletContext对象都是同一个对象
     */
        ServletContext ctx = getServletContext();
    /*
     *	ServletContext 代表Servlet的工作环境，也就是代表当前的Web Server
     *	getServerInfo() 方法可以获得当前服务器的 名称和版本
     */
        String str = ctx.getServerInfo();
        System.out.println(str);
        response.setContentType("text/html;charset=UTF-8");
        response.getWriter().print("OK"); 
    }
}
```

### 数据共享范围（scope）
共享数据方法： setAttribute， getAttribute，removeAttribute
1. pageContext 可以共享数据
2. request 可以共享数据
3. session 可以共享数据
4. servletContext == application 可以共享数据
**如上4种对象其共享数据范围不同。**

1. pageContext 简称 page 范围，其共享范围仅限于 JSP 页面内部，相当于JSP内部的局部变量。
2. request 范围，是一次请求过程范围
3. session 范围，是一次用户会话过程范围，浏览器不关闭与服务器的多次请求的范围。
4. application 范围，是最大的共享范围，一个Web应用的全体Servlet、JSP共享的范围。 适合保存公共静态数据。

使用建议：
1. 数据共享范围越小越好，最常用的是 request
2. 与当前用户有关的数据，共享到 session，比如用户登录状态
3. 全局数据，可以共享到 application

EL表达式，会自动的按照由小到大搜索数据:
```
    ${root} 检索顺序
    pageContext.getAttribute("root");
    request.getAttribute("root");
    session.getAttribute("root");
    application.getAttribute("root");
```

利用session数据范围，在页面上显示登录用户信息：
更新header.jsp:
```
	${loginUser.name}
```