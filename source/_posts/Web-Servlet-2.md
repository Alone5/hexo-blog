---
title: Web-Servlet-2
date: 2019-07-09 17:30:46
tags: github
---
### Request 接收表单参数
Request功能很多, 其主要作用就是处理浏览器请求. 用户通过表单提交的参数可以通过Request对象提供的API方法读取
```java
String name = request.getParamter("name");
```

案例:
1. 编写表单 webapp/login.html
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>登录表单</title>
</head>
<body>
    <h1>登录</h1>
    <p>使用request对象接收表单参数</p>
    <!-- 向 login url发起post请求 -->
    <form action="login" method="post">
        <div>
            <label>用户</label>
            <input type="text" name="user"> 
        </div>
        <div>
            <label>密码</label>
            <input type="password" name="pwd"> 
        </div>
        <input type="submit" value="登录">   
    </form>
</body>
</html>
```

2. 测试显示表单

3. 编写Servlet
```java
public class LoginServlet extends HttpServlet {
     private static final long serialVersionUID = 1L;
     protected void doPost(HttpServletRequest request,HttpServletResponse response) throws ServletException, IOException {
         //利用request的API获取表单参数
         String name = request.getParameter("user");
         String pwd = request.getParameter("pwd");
         System.out.println(name+","+pwd);
 
         response.setContentType("text/html; charset=utf-8"); 
         PrintWriter out=response.getWriter();
         out.print("OK"); 
     }
 }
```

#### 参数编码问题
1. 网络和IO 每次只能传输一个字节(8位)信息!
2. 字符是16位无符号整数
3. 利用网络传输字符时候, 需要将字符进行拆分才能传输
4. 字符的拆分方案称为字符的编码!

Post请求时候, 在获取请求参数之前设置请求的编码可以解决编码问题.
案例:
```java
public class LoginServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;
    protected void doPost(HttpServletRequest request,HttpServletResponse response)throws ServletException, IOException {
        //在读取参数之前设置request的编码,处理解码问题
        request.setCharacterEncoding("UTF-8");
        //利用request的API获取表单参数
        String name = request.getParameter("user");
        String pwd = request.getParameter("pwd");
        System.out.println(name+","+pwd);
        response.setContentType("text/html; charset=utf-8"); 
        PrintWriter out=response.getWriter();
        out.print("OK"); 
    }
}
```

#### 接收get请求参数
getParamter不仅可以接受post请求参数, 也可以接收get请求参数.
get编码问题可以在Tomcat配置文件server.xml中配置:
```xml
<Connector URIEncoding="UTF-8" connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443"/>
```

案例:
```java
public class GetDemoServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //接收浏览器通过 get 请求发送的参数
        //如果浏览器没有提供对应的参数, 则接收到null
        String id=request.getParameter("id");
        String name=request.getParameter("name");
        System.out.println(id+", "+name);
        response.setContentType("text/html;charset=UTF-8");
        PrintWriter out = response.getWriter();
        out.print("OK");
    }
}
```

配置:
```xml
<servlet>
  <description></description>
  <display-name>GetDemoServlet</display-name>
  <servlet-name>GetDemoServlet</servlet-name>
  <servlet-class>day03.GetDemoServlet</servlet-class>
</servlet>
<servlet-mapping>
  <servlet-name>GetDemoServlet</servlet-name>
  <url-pattern>/get-demo</url-pattern>
</servlet-mapping>
```

## 利用Servlet将数据库中的表显示到浏览器中

#### 1. 创建表
创建员工表:
```mysql
create database web;
use web;
create table t_emp(
    empno int auto_increment primary key,
    ename varchar(100),
    mgr int,
    deptno int,
    hiredate datetime,
    salary double,
    comm double
);
insert into t_emp (empno, ename, mgr, deptno, hiredate, salary, comm)
values (null, 'Tom', 0, null, now(), 1000, 200);

insert into t_emp (empno, ename, mgr, deptno, hiredate, salary, comm)
values (null, 'Jerry', 1, null, now(), 2000, 100);
```

#### 2. 编写数据库连接配置文件 db.properties
```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/web?characterEncoding=utf8&useUnicode=true&useSSL=false
username=root
password=
```

#### 3. 编写数据库连接工具
```java
public class DBUtil {
    private static String driver;
    private static String url;
    private static String username;
    private static String password;
    static {
        try {
            InputStream in = DBUtil.class.getClassLoader().getResourceAsStream("db.properties");
            Properties cfg = new Properties();
            cfg.load(in);
            in.close();
            driver = cfg.getProperty("driver");
            url = cfg.getProperty("url");
            username=cfg.getProperty("username");
            password=cfg.getProperty("password");
        }catch(Exception e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }

    public static Connection getConnection() throws Exception{
        Class.forName(driver);
        Connection conn = DriverManager.getConnection(url,username,password);
        return conn;
    }

    public static void close(Connection conn) {
        try {
            if(conn!=null) {
                conn.close();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

#### 4. 利用JUnit进行测试:
1. 导入JUnit
```xml
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.12</version>
</dependency>
```

2. 编写测试案例:
```java
public class TestCase {
    @Test
    public void test() {
        System.out.println("Hello World!"); 
    }

    @Test
    public void testHello() {
        System.out.println("Hello Kitty!"); 
    }

    @Test
    public void testGetConnection() {
        String sql = "select now() d";
        Connection conn = null;
        try {
            conn = DBUtil.getConnection();
            Statement st = conn.createStatement();
            ResultSet rs = st.executeQuery(sql);
            while(rs.next()) {
                Date date=rs.getDate("d");
                System.out.println(date);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally{
            DBUtil.close(conn);
        }
    }
}
```

#### 5. 编写Servlet 显示表内容
代码:
```java
public class ListServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获得out对象
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();

        out.println("<!DOCTYPE html>");
        out.println("<html>");
        out.println("<head>");
        out.println("<meta charset=\"UTF-8\">");
        out.println("<title>登录表单</title>");
        out.println("</head>");
        out.println("<body>");
        out.println("<h1>员工列表</h1>"); 

        String sql="select empno, ename, mgr, hiredate, deptno, salary, comm from t_emp";
        Connection conn = null;
        try {
            conn = DBUtil.getConnection();
            Statement st = conn.createStatement();
            ResultSet rs = st.executeQuery(sql);
            //输出表格头
            out.println("<table>");
            out.println("<tr>");
            out.println("<td>编号</td>");
            out.println("<td>姓名</td>");
            out.println("<td>入职日期</td>");
            out.println("</tr>");
            while(rs.next()) {
                int empno = rs.getInt("empno");
                String ename = rs.getString("ename");
                Date date = rs.getDate("hiredate");
                //System.out.println(empno+","+ename);
                out.println("<tr>");
                out.println("<td>"+empno+"</td>");
                out.println("<td>"+ename+"</td>");
                out.println("<td>"+date+"</td>");
                out.println("</tr>");
            }
            out.println("</table>");
        }catch (Exception e) {
            e.printStackTrace();
        } finally {
            DBUtil.close(conn);
        }
        out.println("</body>");
        out.println("</html>"); 
    }
}
```

配置:
```xml
<servlet>
  <description></description>
  <display-name>ListServlet</display-name>
  <servlet-name>ListServlet</servlet-name>
  <servlet-class>day03.ListServlet</servlet-class>
</servlet>
<servlet-mapping>
  <servlet-name>ListServlet</servlet-name>
  <url-pattern>/list</url-pattern>
</servlet-mapping>
```

测试:
```
http://localhost:8080/Servlet03/list
```

