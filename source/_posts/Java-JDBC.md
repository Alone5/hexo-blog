---
title: Java-JDBC
date: 2019-06-26 18:24:07
tags: github
---
## JDBC
- Java 数据库连接: 将Java程序连接到数据的桥梁.
1. Sun(Java) 设计了JDBC API底层封装了Socket, 简化数据库的访问.
2. JDBC为数据库提供了统一访问接口.

- 使用JDBC
1. 利用maven导入数据库驱动
	- 创建Maven项目
	- 找到 JDBC驱动程序的 坐标
	- 将JDBC坐标添加到 pom.xml
2. 注册数据库驱动: 告诉JDBC如何找到数据库驱动的实现类
	- 最新的数据库驱动,会自动注册(有些驱动程序不支持).
	- 建议手动注册:
		1. Class.forName("数据库驱动程序类名")
		2. Class.forName("com.mysql.jdbc.Driver")
3. 建立与数据库之间的连接
   ```java
   String usr="root";
   String pwd="";
   //          jdbc:mysql://数据库IP:3306/数据库名    
   String url="jdbc:mysql://localhost:3306/db6";
   Connection conn=DriverManger.getConnection(url, usr, pwd);
   ```
4. 创建Statement(语句)对象: 用于执行SQL(操作数据库)
	1. DDL create drop 等 一般使用 execute
	2. DML insert delete update 一般使用 executeUpdate 执行
	3. DQL select 一般使用 executeQuery
	```java
	String sql = "create table t_user(id int, name varchar(50))";
	Statement st = conn.createStatement();
	st.execute(sql);
	st.close();
	```
5. 关闭连接
   ```
   conn.close();
   ```

- 建表案例:
	```java
public static void main(String[] args) 
    throws Exception{
    //注册数据库驱动
    Class.forName("com.mysql.jdbc.Driver");
    //建立连接
    String usr="root";
    String pwd="root";
    String url="jdbc:mysql://localhost:3306/db6";
    Connection conn = 
            DriverManager.getConnection(url,usr,pwd);
    //测试:
    System.out.println(conn);  
    //创建Statement对象,执行SQL
    Statement st = conn.createStatement();
    String sql = "create table t_user ("
            + "id int, "
            + "name varchar(100))";
    //执行sql语句
    st.execute(sql);
    st.close();
    //关闭连接
    conn.close();
}
	```

- 插入数据案例:
	```java
public static void main(String[] args) 
    throws Exception {
    /**
     * 利用JDBC执行插入语句
     */
    String sql="insert into t_user "
            + "(id, name) "
            + "values (2, '范传奇')"; 
    //注册驱动
    Class.forName("com.mysql.jdbc.Driver");

    //连接数据库
    String url="jdbc:mysql://localhost:3306/db6?characterEncoding=utf8&useUnicode=true&useSSL=false";
    String username="root";
    String password="root";
    Connection conn = DriverManager.getConnection(
        url, username, password);
    //创建 Statement对象
    Statement st = conn.createStatement();
    //executeUpdate 返回数据库中更新行数!
    int n = st.executeUpdate(sql);
    //处理SQL结果
    if(n==1) {
        System.out.println("插入成功!");
    }
    //关闭资源和连接
    st.close();
    conn.close();
}
	```

- MySQL JDBC 连接常用参数, 写在连接url上:

1. characterEncoding 字符编码, 可以设置为utf8

2. useUnicode 是否使用unicode字符编码, 设置为true

3. 关闭ssl加密, useSSL 设置为false
	```java
		jdbc:mysql://localhost:3306/db6?						characterEncoding=utf8&useUnicode=true&useSSL=false
	```
- 删除案例
	```java
public static void main(String[] args) 
    throws Exception {
    /**
     * 利用JDBC执行删除语句
     */
    String sql="delete from t_user where id=1";  
    //注册驱动
    Class.forName("com.mysql.jdbc.Driver");
    //连接数据库
    String url="jdbc:mysql://localhost:3306/db6";
    String username="root";
    String password="root";
    Connection conn = DriverManager.getConnection(
        url, username, password);
    //创建 Statement对象
    Statement st = conn.createStatement();
    //executeUpdate 返回数据库中更新行数!
    int n = st.executeUpdate(sql);
    //处理SQL结果
    if(n>=1) {
        System.out.println("删除成功!");
    }else {
        System.out.println("删除失败!");
    }
    //关闭资源和连接
    st.close();
    conn.close();
}
	```

