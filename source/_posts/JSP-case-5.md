---
title: JSP-case-5
date: 2019-07-16 19:51:32
tags: github
---
## 重构员工项目
1. 添加 emp.EmpContoller 控制器类
```java
   public class EmpController {
       @RequestMapping("/emp/list.do")
       public String list(HttpServletRequest request) {
           EmpDao dao = new EmpDao();
           List<Emp> list = dao.findAll();
           Map<Integer, String> names=new HashMap<>();
           for (Emp emp : list) {
               names.put(emp.getEmpno(), emp.getEname());
           }
           //利用request对象将数据共享到JSP页面
           request.setAttribute("list", list);
           request.setAttribute("names", names);
           //转发的员工列表页面
           return "list-emp2";
       }
   
       @RequestMapping("/emp/add.do")
       public String add(HttpServletRequest request) {
           EmpDao dao = new EmpDao();
           List<Emp> mgrs = dao.findMgrs();
           request.setAttribute("mgrs", mgrs);
           return "add-emp";
       }
       @RequestMapping("/emp/save.do")
       public String save(HttpServletRequest request) {
           try {
               request.setCharacterEncoding("UTF-8");
               String ename=request.getParameter("ename");
               String mgrId=request.getParameter("mgr");
               String date =request.getParameter("hiredate");
               String deptId=request.getParameter("deptno");
               String salarys=request.getParameter("salary");
               String comms = request.getParameter("comm");
   
               //数据类型转换: 将字符串转换为目标数据类型
               SimpleDateFormat fmt= new SimpleDateFormat("yyyy-MM-dd");
               int mgr = Integer.parseInt(mgrId);
               Date hiredate = new Date(fmt.parse(date).getTime());
               int deptno = Integer.parseInt(deptId);
               double salary = Double.parseDouble(salarys);
               double comm = Double.parseDouble(comms);
               //保存到数据库
               Emp emp = new Emp(0, ename, mgr, hiredate, deptno, salary, comm);
               EmpDao dao = new EmpDao();
               int n = dao.save(emp);
               if(n==1) {
                   //设置绝对重定向路径
                   String url= request.getContextPath()+"/emp/list.do";
                   return "redirect:"+url;
               }else {
                   //失败
                   request.setAttribute("message", "添加失败!");
                   return "message";
               }
           }catch(Exception e) {
               e.printStackTrace(); 
               throw new RuntimeException(e);
           }
       }
       @RequestMapping("/emp/delete.do")
       public String delete(HttpServletRequest request) {
           String no = request.getParameter("empno");
           //请自行打桩测试接收到的参数是否有效
           int empno = Integer.parseInt(no);
           //调用EmpDao删除数据
           EmpDao dao = new EmpDao();
           int n = dao.delete(empno);
           if(n==1) {
               //重定向到 员工列表页面, 显示结果
               String path=request.getContextPath()+"/emp/list.do";
               return "redirect:"+path;
           } else {
               //转发到message.jsp显示错误消息
               request.setAttribute("message", "删除失败");
               return "message";
           }
   
       }
       @RequestMapping("/emp/edit.do")
       public String edit(HttpServletRequest request) {
           String no = request.getParameter("empno");
           int empno = Integer.parseInt(no);
           EmpDao dao = new EmpDao();
           Emp emp = dao.findByEmpno(empno);
   
           List<Emp> mgrs = dao.findMgrs();
   
           request.setAttribute("mgrs", mgrs);
           request.setAttribute("emp", emp);
   
           return "edit-emp";
       }
       @RequestMapping("/emp/update.do")
       public String update(HttpServletRequest request) {
           try {
               request.setCharacterEncoding("UTF-8");
               String no = request.getParameter("empno");
               String ename=request.getParameter("ename");
               String mgrno=request.getParameter("mgr");
               String hire=request.getParameter("hiredate");
               String dept=request.getParameter("deptno");
               String sly =request.getParameter("salary");
               String com =request.getParameter("comm");
               int empno = Integer.parseInt(no);
               int mgr = Integer.parseInt(mgrno);
               //java.sql.Date 提供了将字符串转换为日期的方法
               Date hiredate = Date.valueOf(hire);
               int deptno = Integer.parseInt(dept);
               double salary = Double.parseDouble(sly);
               double comm = Double.parseDouble(com);
   
               Emp emp = new Emp(empno, ename, mgr, hiredate, deptno, salary, comm);
               EmpDao dao = new EmpDao();
               //更新数据
               int n = dao.updateEmp(emp);
               if(n==1) {
                   String path=request.getContextPath()+"/emp/list.do";
                   return "redirect:"+path; 
               } else {
                   request.setAttribute("message", "更新失败！");
                   return "message";
               }
           } catch (Exception e) {
               e.printStackTrace();
               throw new RuntimeException(e); 
           }
       }
   }
```

2. 添加 emp.UserContoller 控制器类
```java
   public class UserController {
       @RequestMapping("/user/login.do")
       public String login(HttpServletRequest request) {
           //读取表单参数
           String name = request.getParameter("name");
           String password = request.getParameter("password");
           String save = request.getParameter("save");
   
           // 访问数据层，检查用户信息
           UserDao dao = new UserDao();
           User user = dao.findUserByName(name.trim());
   
           //将用户名保存在 request中
           request.setAttribute("name",name);
   
           //String contextPath = request.getContextPath();
           //request.setAttribute("root", contextPath); 
   
           //如果没有找到用户信息，表示用户名是错误的
           if(user==null) {
               //转回到登录页面显示错误消息
               request.setAttribute("message", "用户名或者密码错误"); 
               return "login";
           }
           //找到用户信息，则判断密码是否正确
           //哪个变量不可能为null放到前面
           if( user.getName().equals(name) && 
               user.getPassword().equals(password)) {
               //如果密码相等， 则可以登录
               //将登录结果保存在 session 
               HttpSession session = request.getSession();
               session.setAttribute("loginUser", user);
   
               //转发到消息页面显示登录成功消息
               request.setAttribute("message", "登录成功");
               return "message";
           }
           //执行到这个位置？密码不一致的情形
           request.setAttribute("message", "用户名或者密码错误");
           return "login";
       }
       @RequestMapping("/user/start-login.do")
       public String startLogin(HttpServletRequest request) {
           return "login";
       }
   }
```

3. 配置 beans.xml
```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans>
       <bean class="mvc.BizController"></bean>
       <bean class="emp.EmpController"></bean>
       <bean class="emp.UserController"></bean>
   </beans>
```

4. 更新 所有JSP页面，将链接增加 .do
5. 更新 AccessFilter
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
               String login=req.getContextPath()+"/user/start-login.do";
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