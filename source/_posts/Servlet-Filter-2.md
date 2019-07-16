---
title: Servlet-Filter-2
date: 2019-07-16 18:48:16
tags: github
---
## 监听器
#### 1、什么是监听器？
Java EE 设计了一系列回调函数接口，在Web服务器的特定工作节点时候，执行回调方法。 利用这些方法可以在服务器特定工作点插入功能。

监听器是Servlet规范中定义的一种特殊类，用于监听ServletContext、HttpSession和ServletRequest等域对象的创建和销毁事件，它还可以监听域对象的属性发生修改的事件，可以在事件发生前或者发生后做一些必要的处理。

在Servlet中要创建监听器类首先需要新建一个类并继承相应的监听器接口，实现接口中定义的方法，然后在web.xml文件中注册相应的监听器即可。如果一个web.xml文件中注册了多个监听器，则监听器的启动顺序按照在web.xml中的注册顺序启动。如果一个web.xml文件中同时定义了监听器、过滤器和Servlet，那么web容器会先加载监听器、再加载过滤器最后加载Servlet。

**示例：**
利用监听器检测 ServletContext 对象何时创建，何时销毁：
使用监听器, ServletContextListener 监听器的使用。
1. 实现 ServletContextListener 接口；
2. 在web.xml 中配置 Listener，使监听器生效。

1. 创建监听器对象:
```java
   public class ContextDemoListener implements ServletContextListener{
       // context 上下文， Initialized 初始化完成了
       // 此方法会在 ServletContext对象初始化完成以后执行
       @Override
       public void contextInitialized(ServletContextEvent e) {
           System.out.println("初始化完成");
       }
       // Destroyed 销毁完成了
       // 此方法会在ServletContext对象销毁完成以后执行
       @Override
       public void contextDestroyed(ServletContextEvent e) {
           System.out.println("销毁以后");
       }
   }
```

2. 配置 web.xml
```xml
   <listener>
       <listener-class>day11.ContextDemoListener</listener-class>
   </listener>
```

3. 测试： 由于 ServletContext对象在容器启动时候创建，在容器关闭时候销毁，所以在容器启动时候和关闭时候能够看到打桩语句的输出。
ServletContextListener 可以用于初始化或者销毁全局资源。
1. 如初始化：在ServletContext对象初始化以后，立即绑定 root=ContextPath 更加合理。
2. 如销毁：在ServletContext对象销毁以后，关闭数据库连接池，保证数据库连接池可靠关闭所有连接。

1. 创建Listener
```java
/**
 * 初始化和销毁应用程序的全局资源
 * 1. ServletContext 创建时候 初始化 root=ContextPath
 * 2. ServletContext 销毁时候 关闭数据库连接池
 */
public class ResourceInitDestroyListener implements ServletContextListener {
    public void contextInitialized(ServletContextEvent e)  { 
        ServletContext ctx = e.getServletContext();
        String contextPath = ctx.getContextPath();
        ctx.setAttribute("root", contextPath); 
        System.out.println("初始化了 root="+contextPath); 
    }

    public void contextDestroyed(ServletContextEvent e)  { 
        //关闭数据库连接池
        DBUtil.close(); 
        System.out.println("关闭了数据库连接池！"); 
    }
}
```

2. 配置 web.xml
```xml
    <listener>
      <listener-class>day11.ResourceInitDestroyListener</listener-class>
    </listener>
```

### Servlet线程安全问题
1. Servlet对象是单例的，其service（包含被其调用的doGet、doPost）方法是被Tomcat线程并发调用的。
2. service方法是存在线程安全问题。
3. 一般情况下，不要在Servlet中定义可读写变量！保持Servlet是无状态的类。 “定义无状态的Servlet”
	1. 无状态的类： 没有可以读写变量的类称为无状态类！可以有只读变量。
4. 如果定义了有状态的Servlet，进行适当同步（加锁），解决线程安全问题。

案例：
```java
public class SetNameServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    private String name;
    /**
     * 线程并发安全性演示
     */
    protected void doGet(
        HttpServletRequest request, 
        HttpServletResponse response) 
        throws ServletException, IOException {
        //获取执行当前doGet方法的线程，这个线程
        //由Tomcat的线程池管理，用于处理当前http请求
        Thread t = Thread.currentThread();
        System.out.println(t); 
		
		//解决线程安全问题
        synchronized (this) {
            String n = request.getParameter("n");
            name = n;

            try {
                Thread.sleep(5000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            response.setContentType("text/html;charset=UTF-8");
            response.getWriter().println("name:"+name); 
        }
    }
}
```
测试：打开两个浏览器：
```
	localhost:8080/Servlet/SetNameServlet?name=Tom
	//name分别输入不同的值
```
