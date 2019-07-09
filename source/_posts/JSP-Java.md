---
title: JSP-Java
date: 2019-07-09 17:30:46
tags: github
---

## JSP

JSP: Java服务器页面
```
J-Java
S-Server
P-Page
```
1. 历史上 微软公司首先提供了 ASP 技术, 很受欢迎
2. SUN根据ASP的风格基于Servlet技术, 设计了JSP
3. JSP就是Servlet

JSP编程好处:
1. JSP 页面基于HTML页面, 传统的HTML开发工具可以写JSP
2. 在运行时候 JSP 被翻译成 .java 文件(Servlet) 再编译为.class
3. 执行时候执行class文件.
4. JSP自动部署, 不用写web.xml
   
   > JSP 的优势在于 使用HTML风格编码编写Servlet类! JSP非常适合显示网页符数据.

**JSP Hello World!**
```jsp
<%@ page language="java" contentType="text/html; charset=utf-8" pageEncoding="utf-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>第一个JSP</title>
</head>
<body>
    <h1>第一个JSP</h1>
    <!-- JSP 中可以嵌入 "Java 程序脚本" -->
    <%
        String str = "Hello World!";
    %>
    <!-- JSP 表达式, 用于计算表达式值 -->
    <p><%=str%></p>
</body>
</html>
```

## JSP运行原理
JSP运行过程:

- 第一次请求JSP时候
  1. 先将JSP翻译为一个 Servlet 源程序(.java)
  2. 再将Servlet源程序编译为.class文件
  3. 创建Servlet对象
  4. 执行Servlet对象的 service 方法, 处理请求与响应
- 第二次请求JSP时候
  1. 如果JSP被编辑过则重新走流程 1
  2. 如果JSP没有被编辑过, 则执行执行Servlet对象的 service 方法

## JSP 中的Java
1. Java 脚本 : 用于处理程序逻辑, 可以写任意语句, 流程控制等
```jsp
<%
    //Java 脚本 
    long now = System.currentTimeMillis();
    long year = now/1000/60/60/24/365 + 1970;
%>
```
> Java 脚本中的语句会翻译到 service 方法中的语句
2. Java 表达式: 用于计算并且输出表达式
```jsp
<!-- JSP 表达式, 用于计算表达式值并且在网页上输出-->
<p><%=now/1000%60%></p>
<p><%=year%></p>
<p><%=add(times, 1)%></p>
```

3. Java 声明: 用于为Servlet声明成员
```jsp
<!-- JSP 中的"声明",  -->
<%!
    //用于为当前类声明 属性/方法/静态成员等
    //编译以后会成为 Servlet 中的成员
    int times = 55;
    int add(int a, int b){
        return a+b;
    }
%>
```

案例:
```jsp
<%@ page language="java" 
    contentType="text/html; charset=utf-8"
    pageEncoding="utf-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Java 程序</title>
</head>
<body>
    <h1>JSP中的Java</h1>
    <!-- JSP 中的"声明",  -->
    <%!
        //用于为当前类声明 属性/方法/静态成员等
        //编译以后会成为 Servlet 中的成员
        int times = 55;
        int add(int a, int b){
            return a+b;
        }
    %>
    <%
        //Java 脚本 
        long now = System.currentTimeMillis();
        long year = now/1000/60/60/24/365 + 1970;
    %>
    <!-- JSP 表达式, 用于计算表达式值并且在网页上输出-->
    <p><%=now/1000%60%></p>
    <p><%=year%></p>
    <p><%=add(times, 1)%></p>
</body>
</html>
```



## page 指令
使用 <%@ page %> 声明page 指令
1. 必须写在JSP的第一行
2. 属性:
   - language=java 可以省略, 省略就是Java, 只能Java
   - contentType=”text/html; charset=UTF-8” 就是 response.setContentType(…) 设置浏览器端的解析编码
   - pageEncoding=”UTF-8” 页面编码, 是告诉Tomcat读取当前网页时候的编码
   - import=”java.util.*,java.sql.*“ 导入组件

## JSP内置对象
- 就是JSP中预先定义的引用, 引用了与JSP功能相关的对象.
- 可以随时使用引用调用对象的方法, 实现功能
- 一共有9个对象: 经常成为9大内置对象.
常用:
1. request HttpServletResuest
2. response HttpServletResponse
3. out JspWriter 功能与 PrintWriter 一致

> 考点: JSP out对象的类型: JspWriter

比较常用:
1. session HttpSession 类型, 用于存储与”用户会话”有关的数据.(以后讲)
2. application ServletContext 类型, 用于存储与当前Servlet容器有关的数据(后续专门讲)
3. pageContext PageContext, 页面”上下文Context”. PageContext是当前页面上下文, 是当前页面的大管家, 内部包含与JSP有关的全部资源.
```bash
	pageContext.getRequest() 返回的对象就是 request
	pageContext.getResponse()  返回的对象就是 response对象
```

不常用:
1. config : ServletConfig 用于读取 web.xml 的内容
2. page : Object类型, 是当前JSP对象(Servlet对象)的引用, 就是this
3. exception : Exception 类型, 当前网页是错误页面时候, 用于获取异常信息.

案例:
```jsp
<%@ page 
    language="java" 
    contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    import="java.util.*,java.sql.*" 
%>
<%
    List<String> list=new ArrayList<String>();
    //演示request对象的功能: 读取请求中的数据
    String ua = request.getHeader("User-Agent");
    //演示response对象功能: 处理响应信息
    response.setHeader("message", "Hello World!"); 
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    <h1>内置对象功能演示</h1>
    <p>User-Agent:
    <%
        //在p标签里面显示ua信息
        out.println(ua); 
    %>
    </p>
    <%
        //pageContext 页面上下文, 包含当前页面全部相关信息
        // pageContext.getOut() 返回对象就是 out 对象
        // 其功能与out对象相同
        JspWriter o = pageContext.getOut();
    %>
    <p>User-Agent: <% o.println(ua); %> </p>
</body>
</html>
```

## 利用request对象共享数据
request内部封装了一个集合, 用于在一个请求期间共享数据。
主要用于在Servlet与JSP之间共享数据。
其主要API方法:
```java
    request.setAttribute(key, value);
    Object value = request.getAttribute(key);
    request.removeAttribute(key);
```

案例:
```jsp
<%@ page 
    language="java" 
    contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    import="java.util.*,java.sql.*" 
%>
<%
    //将数据存储到 request中
    request.setAttribute("msg", "Hello World!");
    request.setAttribute("price", 1234.0);
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    <h1>利用request共享/传送数据</h1>
    <%
        //接收request传递/共享的数据
        String s = (String)request.getAttribute("msg");
        Double d = (Double)request.getAttribute("price");
        //删除request中共享的数据
        request.removeAttribute("msg");
        request.removeAttribute("price");
    %>
    <p>msg:<%=s%> price:<%=d%></p>
</body>
</html>
```

## 转发
利用请求协作对象, 从Servlet转发到JSP。
SUN为了能够实现Servlet处理业务功能, 由JSP显示业务处理结果, 而设计了Servlet到JSP的转发功能。
案例:
```java
public class DemoServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //测试 从Servlet转发到JSP
        //1. 保存数据到 request 对象
        request.setAttribute("msg", "Hello World!");
        //2. 利用请求协作对象转发到一个JSP页面
        RequestDispatcher rd=request.getRequestDispatcher("demo03.jsp");
        rd.forward(request, response);
        //注意!! 转发以后不能再处理网页内容, 
        //包括向response输出信息, 或者再次转发!!
    }
}

  <servlet>
    <description></description>
    <display-name>DemoServlet</display-name>
    <servlet-name>DemoServlet</servlet-name>
    <servlet-class>day05.DemoServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>DemoServlet</servlet-name>
    <url-pattern>/demo</url-pattern>
  </servlet-mapping>

<%@ page 
    language="java" 
    contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    import="java.util.*,java.sql.*" 
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
    <h1>从Servlet转发到JSP</h1>
    <p><%=request.getAttribute("msg")%></p> 
</body>
</html>
```

## 实现MVC版本的员工列表:
1. DBUtil, Emp 等略
2. 编写Servlet
```java
public class ListEmpServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //利用Dao获取数据
        EmpDao dao = new EmpDao();
        List<Emp> list = dao.findAll();
        //利用request共享数据
        request.setAttribute("list", list); 
        //转发到 list-emp.jsp, 显示员工列表
        RequestDispatcher rd=request.getRequestDispatcher("list-emp.jsp");
        rd.forward(request, response); 
    }
}
```

3. 配置
```xml
<servlet>
  <description></description>
  <display-name>ListEmpServlet</display-name>
  <servlet-name>ListEmpServlet</servlet-name>
  <servlet-class>day05.ListEmpServlet</servlet-class>
</servlet>
<servlet-mapping>
  <servlet-name>ListEmpServlet</servlet-name>
  <url-pattern>/list-emp</url-pattern>
</servlet-mapping>
```

4. 编写JSP
```jsp
<%@ page 
    language="java" 
    contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    import="java.util.*,entity.*"
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>员工列表</title>
</head>
<body>
    <h1>员工列表</h1>
    <table border="1">
        <tr>
            <td>编号</td>
            <td>姓名</td>
            <td>领导</td>
            <td>入职日期</td>
            <td>部门号</td>
            <td>薪资</td>
            <td>提成</td>
        </tr>
<% 
    List<Emp> list = 
        (List<Emp>)request.getAttribute("list");
%>
        <%for(Emp emp:list){ %>
        <tr>
            <td><%=emp.getEmpno()%></td>
            <td><%=emp.getEname()%></td>
            <td><%=emp.getMgr()%></td>
            <td><%=emp.getHiredate()%></td>
            <td><%=emp.getDeptno()%></td>
            <td><%=emp.getSalary()%></td>
            <td><%=emp.getComm()%></td>
        </tr>
        <%}%>
    </table>
</body>
</html>
```