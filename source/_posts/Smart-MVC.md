---
title: Smart-MVC
date: 2019-07-17 09:26:27
tags: github
---

SmartMVC 是Spring MVC 框架克隆版。
设计目标： 将复杂的Web编程封装起来，使开发更加快速便捷。

### 控制器与业务功能拆分：
1. 创建前端控制器：
```java
   public class DispatcherServlet extends HttpServlet {
       private static final long serialVersionUID = 1L;
   
       protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
           //检查request\response引用的对象类型
           Class cls1 = request.getClass();
           Class cls2 = response.getClass();
           System.out.println(cls1);
           System.out.println(cls2);
   
           //调用业务方法，根据业务方法的返回值转发到JSP
           BizController controller=new BizController();
           //执行业务方法，得到转发的目标页面名称
           String target = controller.execute(request);
           //将目标页面名称前后增加 JSP文件的位置和后缀
           String path = "/WEB-INF/jsp/"+target+".jsp";
           request.getRequestDispatcher(path)
           .forward(request, response); 
       }
   
       protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
           doGet(request, response);
       }
   }
```

2. 配置 web.xml，使前端控制器可以处理任何  * .do 请求
```xml
       <servlet>
           <description></description>
           <display-name>DispatcherServlet</display-name>
           <servlet-name>DispatcherServlet</servlet-name>
           <servlet-class>mvc.DispatcherServlet</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>DispatcherServlet</servlet-name>
           <url-pattern>*.do</url-pattern>
       </servlet-mapping>
```

3. 编写控制器类，封装业务功能
```java
       /**
        * 控制器类， 用于封装业务功能， 
        */
       public class BizController {
           /**
            * 第一个业务功能，Hello World! 
            * @return 目标页面名称
            */
           public String execute(HttpServletRequest request) {
               System.out.println("Hello World!");
               request.setAttribute("msg", "Hello"); 
               return "hello"; 
           }
       }
```

4. 编写 /WEB-INF/jsp/hello.jsp
```jsp
       <%@ page 
         language="java" 
         contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8"%>
       <!DOCTYPE html>
       <html>
       <head>
       <meta charset="UTF-8"> 
       <title>Hello</title>
       </head>
       <body>
         <h1>Hello World！</h1>
         <p>${msg}</p>
       </body>
       </html>
```

5. 测试：
```
   	http://localhost:8080/Servlet12/test.do
```