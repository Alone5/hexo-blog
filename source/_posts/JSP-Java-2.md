---
title: JSP-Java-2
date: 2019-07-10 20:52:58
tags: github
---
## 标准标签库 和 EL表达式
关于标签库 和 EL表达式：
1. 网页中的Java代码不方便维护，经常出现 空指针异常，不友好。
2. SUN提供了自定义标签技术，用于替代Java代码。每个厂商都设计了自己标签。 标签无法通用。自定义标签语法复杂，实现繁琐成本高。
3. Apache组织设计了“标准标签库 JSTL” 实现了绝大部分企业需要用的标签功能。 目前企业基本上都使用标准标签库解决问题。

>使用标准标签库 和 EL表达式

### EL 表达式
EL 表达式有标准标签库一起诞生， 与JSTL配合使用
1. 自动化工作：可以在pageContext，request，session，application中子弹寻找 “attribute”
2. 自动调用 get方法
3. 自动处理空值

HelloWorld:
```jsp
<%@ page 
  language="java" 
  contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"
  import="java.util.*,entity.*"  
%>
<%
  //代表在Servlet中向 request 对象添加的数据
  request.setAttribute("message", "Hello World!");
  Emp emp = new Emp(1, "Tom", 1, null, 1, 1000,100); 
  request.setAttribute("emp", emp);
  request.setAttribute("text", null);
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>EL EL表达式演示</title>
</head>
<body>
  <h1>EL表达式演示</h1>
  <h2>Java 代码输出数据</h2>
<%
  //定义变量，利用getAttribute寻获数据
  String message = 
    (String)request.getAttribute("message");
  Emp one = (Emp) request.getAttribute("emp");
  String text=(String)request.getAttribute("text");
%>  
  <p>利用java表达式输出数据： <%=message%> 
      <%=one.getEname()%> <%=text%> </p>

  <h2>EL表达式输出信息</h2>
  <p>自动寻获 ${message}，
          自动调用get方法 ${emp.ename} 
          自动处理空 ${text} </p>
</body>
</html>
```

#### EL 语法
好处是： 简洁!  写的少，做的多
1. 可以访问对象的 getXxx() 方法
```
   obj.name  就是访问了 obj.getName()
```

2. 可以访问数组、List集合的元素
```
   arr[下标]  
   list[下标]
```

3. 可以访问map
```
   map.key 得到 value
   map['key'] 得到value
```

4. 以上方式的符合使用，无需类型转换
```
   list[1].ename 
   map.east.ename
```

案例：
```jsp
<%@ page 
  language="java" 
  contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"
  import="java.util.*,entity.*"  
%>
<%
  int[] arr1 = {5,1,5,6};
  String[] arr2 = {"Tom","Jerry"};
  Emp[] arr3 = {new Emp(1, "光头强", 1, null, 1,100,10),
          new Emp(1,"李大嘴",1,null,1,100,10)};
  List<String> list1 = new ArrayList<String>();
  list1.add("沉鱼落雁");
  list1.add("明眸善睐");
  List<Emp> list2 = new ArrayList<Emp>();
  list2.add(new Emp(3,"擎天柱",1,null,1,10,1));
  list2.add(new Emp(4,"大黄蜂",1,null,1,20,5));
  Map<String, Emp> map=new HashMap<String, Emp>();
  map.put("east", new Emp(6,"黄老邪",1,null,1,50,1));
  map.put("north", new Emp(7,"洪七公",1,null,1,40,2));

  request.setAttribute("arr1", arr1);
  request.setAttribute("arr2", arr2);
  request.setAttribute("arr3", arr3);
  request.setAttribute("list1", list1);
  request.setAttribute("list2", list2);
  request.setAttribute("map", map);

%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>EL EL表达式演示2</title>
</head>
<body>
  <h1>EL表达式演示2</h1>
  <p>arr1: ${arr1[0]} ${arr1[1]} </p>
  <p>arr2: ${arr2[0]} ${arr2[1]} </p>
  <p>arr3: ${arr3[0].ename} ${arr3[0].empno}</p> 
  <p>list1: ${list1[0]}</p>
  <p>list2: ${list2[0].ename}</p>
  <p>map: ${map.east.ename} ${map['east'].ename}</p>

</body>
</html>
```

### JSTL
使用步骤:
1. 导包
```xml
   <dependency>
       <artifactId>jstl</artifactId>
       <groupId>jstl</groupId>
       <version>1.2</version>
   </dependency>
```

2. 在JSP 导入标签库
```jsp
   <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
```

3. 使用JSP标签
```
   <c:out value="${message}"></c:out>
   out 标签用于输出信息, 自动替换特殊符合，相对输出安全
```

HelloWorld案例：
```jsp
<%@ page 
  language="java" 
  contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"
  import="java.util.*,entity.*"  
%>
<!-- 导入 core 核心标签库，使用前缀是c -->
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%
  request.setAttribute("message", "<Hello World!>"); 
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>JSTL 演示</title>
</head>
<body>
  <h1>JSTL演示</h1> 
  <!-- out 将信息输出到网页中 -->
  <p><c:out value="${message}"></c:out></p>
  <p>${message}</p>
  <!-- JSTL 标签和 EL 表达式都是在服务器上处理的 -->
</body>
</html>
```

#### JSTL 有哪些常用标签：
JSTL标签的设计目的：用于取代浏览器的中的用于显示信息的Java代码
1. out标签，用于输出数据，相对于EL表达式输出，out标签进行了特殊字符替换
2. for 循环标签： 1 计次循环， 2 集合遍历
```jsp
     <c:forEach begin="1" end="5" step="2" var="i"> 
       <p>${i}</p>
     </c:forEach> 
     <ul>
       <c:forEach items="${list}" var="emp" > 
         <li>${emp.empno}, ${emp.ename}</li> 
       </c:forEach>
     </ul>
```

3. if标签 单路分支标签
```jsp
     <c:if test="${active>=100}">
        <span style="color:red">赚钱了</span>
     </c:if>
```

4. 多路分支标签
- choose
- when
- otherwise

```jsp
     <c:choose>
       <c:when test="${active>=200}">
         <span style="color:red">赚大了</span>
       </c:when>
       <c:when test="${active>=100}">
         <span style="color:red">赚钱了</span>  
       </c:when>
       <c:otherwise>
         <span style="color:green">赔本了</span>
       </c:otherwise>
     </c:choose>
```

5. import 包含标
```jsp
   <c:import url="footer.jsp"></c:import>
```

> 重要：JSP、JSP中的脚本、JSP中的指令（包含）、EL、JSTL、等都是在服务器端执行的。 执行结束以后将生成HTML发送到浏览器显示结果。

案例:
```jsp
<%@ page 
  language="java" 
  contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"
  import="java.util.*,entity.*"  
%>
<!-- 导入 core 核心标签库，使用前缀是c -->
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>JSTL 演示</title>
</head>
<body>
  <h1>JSTL演示</h1> 
  <h2>forEach 标签</h2>
  <!-- begin: 开始， end：结束 step: 步子 var:变量名
   如下案例：从1开始循环， 每次增加2， 到5结束循环-->
  <c:forEach begin="1" end="5" step="2" var="i"> 
    <p>${i}</p>
  </c:forEach>
  <h2>集合的遍历</h2>
  <%
    //在实际开发中，如下数据应该在Servlet中生成
    List<String> names = new ArrayList<String>();
    names.add("佟湘玉");
    names.add("白展堂");
    names.add("莫小贝");
    names.add("李大嘴");
    request.setAttribute("names", names); 

    List<Emp> list=new ArrayList<Emp>();
    list.add(new Emp(1, "霸波奔", 1, null, 1, 100,10));
    list.add(new Emp(2, "奔波霸", 1, null, 1, 200,10));
    request.setAttribute("list", list); 
  %>  
  <!-- forEach 遍历集合 -->
  <ul>
    <c:forEach items="${names}" var="name"> 
      <li>${name}</li>
    </c:forEach>
  </ul>
  <h2>遍历对象集合</h2>
  <ul>
    <c:forEach items="${list}" var="emp" > 
      <li>${emp.empno}, ${emp.ename}</li> 
    </c:forEach>
  </ul>
  <h2>if 标签</h2>
  <!-- if 标签只能实现单路分支 -->
  <%
    request.setAttribute("active", 100);
  %>
  <c:if test="${active>=100}">
     <span style="color:red">赚钱了</span>
  </c:if>
  <h2>多路分支标签</h2>
  <!-- 多路分支包含其他情形，相当于else 
    choose 选择 when:当，当xxx的时候，otherwise其他情况 -->
  <c:choose>
    <c:when test="${active>=200}">
      <span style="color:red">赚大了</span>
    </c:when>
    <c:when test="${active>=100}">
      <span style="color:red">赚钱了</span>  
    </c:when>
    <c:otherwise>
      <span style="color:green">赔本了</span>
    </c:otherwise>
  </c:choose>
  <h2>包含标签</h2>
  <!-- 其功能与  jsp:include  一样 -->
  <c:import url="footer.jsp"></c:import>

</body>
</html>
```

## 利用JSTL和EL重构 list-emp2.jsp:
1. 导入标签库：
```jsp
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

2. 更新头部组件包含
```jsp
     <!-- Main Header 包含子页面  -->
     <c:import url="/WEB-INF/include/header.jsp"/>
   
     <!-- Left side 包含子页面 -->
     <c:import url="/WEB-INF/include/left-side.jsp"/>
```

3. 更新集合遍历输出表格
```jsp
   <c:forEach items="${list}" var="emp">             
   <tr>
     <td>${emp.empno}</td>
     <td><c:out value="${emp.ename}"/></td> 
     <td>${emp.mgr}</td>
     <td>${emp.hiredate}</td>
     <td>${emp.deptno}</td>
     <td>${emp.salary}</td>
     <td>${emp.comm}</td>
     <!-- onclick中出现"return false" 则会阻止事件
     -点击事件的响应 -->
     <td><a onclick="return confirm('真的删除?吗?')" 
       href="delete?empno=${emp.empno}" 
       class="btn btn-danger btn-xs"
       ><i class="fa fa-fw fa-remove"></i></a>
       <a href="edit-emp?empno=${emp.empno}"
         class="btn btn-success btn-xs"><i 
         class="fa fa-fw fa-edit"></i></a></td>
   </tr>
   </c:forEach> 
```

4. 更新尾部组件包含
```jsp
     <!-- Main Footer 包含 -->
     <c:import url="/WEB-INF/include/footer.jsp"/>
   
     <!-- Control Sidebar 包含 -->
     <c:import url="/WEB-INF/include/sidebar.jsp"/>
```

## 利用JSTL和EL重构 add-emp.jsp:
1. 导入JSTL
```jsp
   <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%> 
```

2. 替换头部包含标签
```jsp
     <!-- Main Header 包含子页面  -->
     <c:import url="/WEB-INF/include/header.jsp"/>
     <!-- Left side 包含子页面 -->
     <c:import url="/WEB-INF/include/left-side.jsp"/>
```

3. 替换Java代码
```jsp
   <select class="form-control" id="mgr" name="mgr">
     <c:forEach items="${mgrs}" var="mgr">
       <!-- 客户端选中员工名，发送到服务器是对应的员工号
        value属性是发送到服务器上的值 -->
       <option value="${mgr.empno}">
         <c:out value="${mgr.ename}"/>
       </option>
     </c:forEach>
   </select>
```

4. 替换尾部包含
```jsp
   <!-- Main Footer 包含 -->
   <c:import url="/WEB-INF/include/footer.jsp"/>
   <!-- Control Sidebar 包含 -->
   <c:import url="/WEB-INF/include/sidebar.jsp"/>
```

## 利用JSTL和EL重构 edit-emp.jsp:
1. 导入JSTL
```jsp
   <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%> 
```

2. 替换头部包含标签
```jsp
     <!-- Main Header 包含子页面  -->
     <c:import url="/WEB-INF/include/header.jsp"/>
     <!-- Left side 包含子页面 -->
     <c:import url="/WEB-INF/include/left-side.jsp"/>
```

3. 替换Java代码
```jsp
     <!-- Horizontal Form -->
     <div class="box box-info">
       <div class="box-header with-border">
         <h3 class="box-title">添加新员工信息</h3>
       </div>
       <!-- /.box-header -->
       <!-- form start -->
       <form class="form-horizontal" 
         method="post" action="update-emp"> 
         <div class="box-body">
           <div class="form-group">
             <label for="empno" 
               class="col-sm-2 control-label">员工号</label>
             <div class="col-sm-10">
               <input type="text" class="form-control" 
                 id="empno" name="empno" placeholder="员工号"
                 value="${emp.empno}"
                 readonly="readonly" >
             </div>
           </div>
           <div class="form-group">
             <label for="ename" 
               class="col-sm-2 control-label">姓名</label>
             <div class="col-sm-10">
               <input type="text" class="form-control" 
                 id="ename" name="ename" placeholder="姓名"
                 value="<c:out value="${emp.ename}"/>">
             </div>
           </div>
           <div class="form-group">
             <label for="mgr" 
             class="col-sm-2 control-label">领导</label>
             <div class="col-sm-10">
   
               <select class="form-control" id="mgr" name="mgr">
                 <c:forEach items="${mgrs}" var="mgr">
                 <!-- 客户端选中员工名，发送到服务器是对应的员工号
                  value属性是发送到服务器上的值 -->
                   <c:choose>
                     <c:when test="${emp.mgr == mgr.empno}">
                       <option value="${mgr.empno}"
                         selected="selected">
                         <c:out value="${mgr.ename}"/>
                       </option>  
                     </c:when>
                     <c:otherwise>
                       <option value="${mgr.empno}">
                         <c:out value="${mgr.ename}"/>
                       </option>        
                     </c:otherwise>
                   </c:choose> 
                 </c:forEach>
               </select>
             </div>
           </div>
           <div class="form-group">
             <label for="hiredate" 
             class="col-sm-2 control-label">入职日期</label>
             <div class="col-sm-10">
               <input type="date" class="form-control" 
               id="hiredate" name="hiredate"  
               placeholder="入职日期" 
               value="${emp.hiredate}">
             </div>
           </div>
           <div class="form-group">
             <label for="deptno" 
             class="col-sm-2 control-label">部门编号</label>
             <div class="col-sm-10">
               <input type="text" class="form-control" 
               id="deptno" name="deptno"  
               placeholder="部门编号"
               value="${emp.deptno}">
             </div>
           </div>  
           <div class="form-group">
             <label for="salary" 
             class="col-sm-2 control-label">薪资</label>
             <div class="col-sm-10">
               <input type="number" class="form-control" 
               id="salary" name="salary"  
               placeholder="薪资"
               value="${emp.salary}">
             </div>
           </div>  
           <div class="form-group">
             <label for="comm" 
             class="col-sm-2 control-label">提成</label>
             <div class="col-sm-10">
               <input type="number" class="form-control" 
               id="comm" name="comm"  
               placeholder="提成"
               value="${emp.comm}">
             </div>
           </div>                                
         </div>
         <!-- /.box-body -->
         <div class="box-footer">
           <button type="submit" class="btn btn-info pull-right">保存</button>
         </div>
         <!-- /.box-footer -->
       </form>
     </div>
     <!-- /.box -->
```

4. 替换尾部包含
```jsp
   <!-- Main Footer 包含 -->
   <c:import url="/WEB-INF/include/footer.jsp"/>
   <!-- Control Sidebar 包含 -->
   <c:import url="/WEB-INF/include/sidebar.jsp"/>
```

## 显示老板名字问题
1. 重构 ListEmpServlet
```java
   //创建集合，缓存全部的员工号和员工名
   Map<Integer, String> names = new HashMap<>();
   for (Emp emp : list) {
       names.put(emp.getEmpno(), emp.getEname());
   }
   //打桩测试 ...
   request.setAttribute("names", names); 
```
2. 重构 list-emp2.jsp
```
   <td><c:out value="${names[emp.mgr]}"/></td>
```
3. 测试

## 路径问题
Web编程时候有3套路径体系
1. 浏览器视角的Web路径
2. Java Web 用于程序的路径体系
3. 本地操作系统文件路径体系
	- ContextPath路径： Java web应用程序部署在Web服务器时候的部署文件夹。
	- 浏览器视角的路径与Java WEB路径相差了一个ContextPath路径
- 将Java Web路径映射到浏览器视角的Web路径有多种解决方案：
1. 使用相对路径
	- 书写简单，当页面移动显示位置时候会css等资源加载失败出现404错误
2. 利用 <c:url> 标签解决
	- <c:url> 标签就是解决将Java WEB路径转化为 浏览器视角绝对路径而设计的标签
	- 这个标签使用比较繁琐
3. 利用EL表达式在路径前面添加ContextPath，这种方式简洁方便。
	- 在Servlet中将 contextPath保存到request中叫 root
	- 在页面中将所有 html 标签路径修改为 ${root}/...

案例, 将全部emp功能移动到 /emp 中, 目的是为了后续增加权限管理功能
	- 更新全部功能的URL
修改 员工列表功能：

1. 修改web.xml
```xml
   <servlet>
       <description></description>
       <display-name>ListEmpServlet</display-name>
       <servlet-name>ListEmpServlet</servlet-name>
       <servlet-class>day05.ListEmpServlet</servlet-class>
   </servlet>
   <servlet-mapping>
       <servlet-name>ListEmpServlet</servlet-name>
       <url-pattern>/emp/list</url-pattern>
   </servlet-mapping>
```

   > 此时查看网页/emp/list会看到很多 404 错误，导致很多css、js文件没有加载，因为路径上增加了一层emp造成了浏览器视角路径错误，找不到对应资源了。

2. 更新 ListEmpServlet, 将contextPath添加到request中，命名为root
```java
   //将context路径保存request中， 用于将
   //Java WEB目录映射到 浏览器视角WEB目录
   String contextPath = request.getContextPath();
   request.setAttribute("root", contextPath);
```

3. 更新 list-emp2.jsp 将全部html标签中的路径更新为 ${root}/
```jsp
<link rel="stylesheet" href="${root}/bower_components/bootstrap/dist/css/bootstrap.min.css">
<link rel="stylesheet" href="${root}/bower_components/font-awesome/css/font-awesome.min.css">
<link rel="stylesheet" href="${root}/bower_components/Ionicons/css/ionicons.min.css">
<link rel="stylesheet" href="${root}/dist/css/AdminLTE.min.css">
<link rel="stylesheet" href="${root}/dist/css/skins/skin-blue.min.css">

...

<!-- jQuery 3 -->
<script src="${root}/bower_components/jquery/dist/jquery.min.js"></script>
<!-- Bootstrap 3.3.7 -->
<script src="${root}/bower_components/bootstrap/dist/js/bootstrap.min.js"></script>
<!-- AdminLTE App -->
<script src="${root}/dist/js/adminlte.min.js"></script>
```

4. 更新 header.jsp
```jsp
<img src="${root}/dist/img/user2-160x160.jpg" class="img-circle" alt="User Image">
   ...
<img src="${root}/dist/img/user2-160x160.jpg" class="user-image" alt="User Image">
   ...
<img src="${root}/dist/img/user2-160x160.jpg" class="img-circle" alt="User Image">
```

5. 更新 left-side.jsp
```jsp
<img src="${root}/dist/img/user2-160x160.jpg" class="img-circle" alt="User Image">
```

6. 测试 /emp/list

