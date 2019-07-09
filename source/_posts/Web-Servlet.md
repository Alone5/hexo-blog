---
title: Web-Servlet
date: 2019-07-09 17:30:46
tags: github
---

# Servlet

Servlet: 是指在在服务器上执行的程序片段.

1. Servlet是JavaEE规范的一部分
2. 用来解决Web服务器端编程问题
3. Servlet组件工作在Java Web服务器中

## 安装配置Tomcat服务器

1. 下载Tomcat [http://tomcat.apache.org](http://tomcat.apache.org/)
2. 安装: 释放tomcat压缩包即可
3. 在Eclipse中配置Tomcat
   参考: http://doc.tedu.cn/tomcat/index.html

## Servlet Hello World

实现步骤

1. 创建 Maven Web 项目
   1. 创建maven war项目(Eclipse必须能够连接到Mavne仓库)
   2. 在项目上 使用 右键创建 “部署描述文件 web.xml”
2. 导入Tomcat 目标服务器运行环境
   - 就是导入 Servlet 接口
3. 创建一个类 DemoServlet
   - 向response发送 Hello World
```java
public class DemoServlet implements Servlet{
	    public void destroy() {
	    }
	    public ServletConfig getServletConfig() {
	        return null;
	    }
	    public String getServletInfo() {
	        return null;
	    }
	    public void init(ServletConfig arg0) throws ServletException {
	    }
	
	    public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException {
	        response.setContentType("text/html"); 
	        //将 Hello World 写到response中
	        response.getWriter().print("Hello World!");
	    }
}
```

4. 配置web.xml
	1. 将请求路径 /hello 映射到 DemoServlet 上
```xml
<!-- 登记 Servlet -->
<servlet>
  <servlet-name>demo</servlet-name>
  <servlet-class>day01.DemoServlet</servlet-class>
</servlet>
<!-- 将Servlet 映射到 URL 连接 -->
<servlet-mapping>
  <servlet-name>demo</servlet-name>
  <url-pattern>/hello</url-pattern>
</servlet-mapping>
```

5. 将web程序部署到 Tomcat 中
   - 部署就是复制, 将web.xml DemoServlet 等复制到Tomcat服务器中.
6. 使用浏览器请求 /hello, 在浏览器中看到 Hello World!
```
   http://localhost:8080/Servlet01/hello
```

# 请求与响应

### HttpServletRequest
HttpServletRequest 简称 request 对象, 其父类型是 ServletRequest
1. request 对象在Tomcat收到用户请求时候创建, 其内部封装的全部的用户浏览器请求信息.
2. ServletRequest类型定义的方法少, 转换为HttpServletRequest方法更多, 使用更加方便.
3. request对象提供了API方法, 利用这些方法可以读取用户浏览器请求的请求信息.

案例:
1. 创建Servlet类:
```java
	public class RequestDemoServlet implements Servlet{
    public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException {
        //由于getMethod方法是HttpServletRequest对象的方法
        //所以需要将ServletRequest对象强制转型才能调用
        HttpServletRequest req = (HttpServletRequest)request;
        //获取请求行中的请求方式(GET/POST) 
        String method=req.getMethod();
        //获取请求行中的 URI
        String uri = req.getRequestURI();
        //获取请求行中的HTTP版本
        String http = req.getProtocol();
        System.out.println(method+" "+uri+" "+http);
        //获取请求头中的信息
        String host=req.getHeader("Host");
        String ua=req.getHeader("User-Agent");

        System.out.println(host);
        System.out.println(ua);
        //获取全部的请求头信息:
        //Enumeration 与 Iterator 功能相同 
        Enumeration<String> em=req.getHeaderNames();
        //em.hasMoreElements() 与 it.hasNext() 相同
        //em.nextElement();    与 it.next() 相同
        while(em.hasMoreElements()) {
            String name=em.nextElement();
            System.out.println(name+":"+req.getHeader(name));
        }
  
        response.setContentType("text/html");
        response.getWriter().println("OK"); 
    }
    public void destroy() {
    }
    public ServletConfig getServletConfig() {
        return null;
    }
    public String getServletInfo() {
        return null;
    }
    public void init(ServletConfig arg0) throws ServletException {
    }
```

2. 配置web.xml
```xml
<servlet>
  <servlet-name>req</servlet-name>
  <servlet-class>day02.RequestDemoServlet</servlet-class>
</servlet>  
<servlet-mapping>
  <servlet-name>req</servlet-name>
  <url-pattern>/request-demo</url-pattern>
</servlet-mapping>
```

### HttpServletResponse
HttpServletResponse 简称Response 其父类型是 ServletResponse
1. Response用于处理服务器到客户端的响应, 其内部有一个缓冲, 用来保存响应信息.
2. ServletResponse 是父类型, 提供的方法没有 HttpServletResponse 多, 使用没有HttpServletResponse方便. 所以 HttpServletResponse 使用更多.
3. request提供的API用来处理向客户端发送的响应信息

案例:
1. 编写Servlet类
```java
public class ResponseDemoServlet implements Servlet{
    public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException {
        //为了调用子类型的方法, 先进行类型转换
        HttpServletRequest req=(HttpServletRequest)request;
        HttpServletResponse res=(HttpServletResponse)response;
        //利用response对象, 处理对用户浏览器的响应
        //设置响应状态码, 默认状态码就是200
        res.setStatus(200); //400 500 302 等
        //设置响应头, 设置UTF-8支持中文
        res.setContentType("text/html; charset=utf-8");
        String str = "<html><body><h1>今天天气不错!</h1></body></html>";
        byte[] bytes = str.getBytes("utf-8");
        //设置消息正文长度
        res.setContentLength(bytes.length); 
        //添加一个自定义的响应头
        res.setHeader("msg", "Hello World!"); 

        //发送消息正文
        res.getOutputStream().write(bytes); 
        //将response中的信息刷出, 反馈到浏览器
        //如果不调用这个方法, Tomcat会自动调用!
        res.flushBuffer();
    }
    public void destroy() {
    }
    public ServletConfig getServletConfig() {
        return null;
    }
    public String getServletInfo() {
        return null;
    }
    public void init(ServletConfig arg0) throws ServletException {
    }
}
```

2. 配置 web.xml
```xml
<servlet>
  <servlet-name>res</servlet-name>
  <servlet-class>day02.ResponseDemoServlet</servlet-class>
</servlet>
<servlet-mapping>
  <servlet-name>res</servlet-name>
  <url-pattern>/response-demo</url-pattern>
</servlet-mapping>
```

3. 测试: 检查浏览器中收到的响应头信息, 是否包含自定义响应头
   Response提供了更加简便的API, 可以替代如上的API:
4. 采用默认的状态码200
5. 先设置 Content-Type 头, 包含chaeset编码
6. 在创建 PrintWriter对象, printWriter对象会自动处理文本的编码,并且会自动的设置Content-Length

案例:
1. 编写Servlet类
```java
public class ResponseDemo2Servlet implements Servlet{
    public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException {
        HttpServletRequest req = (HttpServletRequest) request;
        HttpServletResponse res = (HttpServletResponse) response;

        //1 响应状态码 200 时候可以不设置, 默认就是200
        //2 设置ContentType响应头, 通知浏览器正文内容和编码
        res.setContentType("text/html;charset=utf-8");
        //3 获取 PrintWriter 对象, 字符流, 封装字符
        //  的编码功能. 按照ContentType设置编码utf-8
        PrintWriter out = res.getWriter();
        //4 设置消息正文内容
        out.print("<html><body><h1>简单版本</h1></body></html>");
        //PrintWriter 会自动处理文字编码,设置ContentLength
    }
    public void destroy() {
    }
    public ServletConfig getServletConfig() {
        return null;
    }
    public String getServletInfo() {
        return null;
    }
    public void init(ServletConfig arg0) throws ServletException {
    }
```

2. 配置web.xml
```xml
<servlet>
  <servlet-name>res2</servlet-name>
  <servlet-class>day02.ResponseDemo2Servlet</servlet-class>
</servlet>
<servlet-mapping>
  <servlet-name>res2</servlet-name>
  <url-pattern>/response2-demo</url-pattern>
</servlet-mapping>
```

### HttpServlet
- 实现Servlet接口创建Servlet非常繁琐, Servlet API 为了简化编程提供了更加简洁的API: HttpServlet.
- HttpServlet实现了Servlet接口, 继承HttpServlet 就间接实现Servlet接口.
- HttpServlet已经完整实现Servlet接口, 只需要简单重写 doGet或者doPost就可以方便的实现Servlet.
- HttpServlet根据用户请求类型, 调用对应的doGet或者doPost方法.

案例:
1. 创建Servlet类
```java
public class HttpDemoServlet extends HttpServlet{
    protected void doGet(HttpServletRequest req,HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html; charset=utf-8");
        PrintWriter out=resp.getWriter();
        out.print("<html><body><h1>");
        out.print("第一个HttpServlet");
        out.print("</h1></body></html>");
    }
}
```

2. 配置web.xml
```xml
<servlet>
  <servlet-name>httpdemo</servlet-name>
  <servlet-class>day02.HttpDemoServlet</servlet-class>
</servlet>
<servlet-mapping>
  <servlet-name>httpdemo</servlet-name>
  <url-pattern>/http-demo</url-pattern>
</servlet-mapping>
```

3. 测试: 可以看出来 继承HttpServlet可以大大简化Servlet编码
   利用Eclipse开发工具提供的Servlet向导可以快速创建Servlet, 这个功能了解即可.

## 概念梳理
URL/URI: 语法规则一致:
```
协议://服务器/路径/资源名
```

1. URL 统一资源定位: 从你计算机开始能够唯一寻获的资源位置, 目标资源一定存在.
   http://cdn.tmooc.cn/bsfile//imgad///4E6DEFE23ED047B2B4AB7AD8B963E9B0.png

2. URI 统一资源识别: 是一个名字, 其对应的位置未必有资源. 目标资源不一定存在, 只是用来作为唯一标识名!!

> 请求行 GET /demo HTTP/1.1 URI

Java WEB 目录结构
1. Java EE 规定了web容器中的WEB应用目录结构
```
webapps    Tomcat WEB应用程序部署目录
    |- examples     Webapp WEB应用
    |   |- WEB-INF
    |   |   |- lib
    |   |   |   |- mysql-jdbc.jar  第三方的jar包
    |   |   |- classes
    |   |   |   |- xxx.class       编写的类文件
    |   |   |- web.xml             部署描述文件
    |   |- index.html
wtpwebapps Eclipse创建了应用程序测试目录, 也是部署目录
    |- Servlet02          Webapp WEB应用
    |   |- WEB-INF
    |   |   |- lib
    |   |   |   |- mysql-jdbc.jar  第三方的jar包
    |   |   |- classes
    |   |   |   |- xxx.class       编写的类文件
    |   |   |- web.xml             部署描述文件
    |   |- index.html

```
> WEB-INF 文件夹是用户不能直接看到的文件夹.
2. Eclipse 会自动在项目中创建目录结构, 在部署时候将目录结构复制到Tomcat中.

Maven 仓库检查步骤
1. 用浏览器检查是否能够看到仓库
   - 检查 https://mirrors.huaweicloud.com/repository/maven/
   - 如果能够看到信息, 说明能够访问仓库, 相反说明网络故障.
2. 修改Eclipse中的Maven settings.xml 文件
```
<mirror>
    <id>huaweicloud</id>
    <mirrorOf>*</mirrorOf>
    <url>https://mirrors.huaweicloud.com/repository/maven/</url>
</mirror>
```

3. 重启Eclipse 检查 Maven Repositories 是否连接到了Maven
4. 在项目上执行 Maven -> Update Project (Force Update)
   - 如果有必须的话, 可以先删除 .m2 文件夹再更新Maven