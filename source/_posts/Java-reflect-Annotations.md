---
title: Java-reflect-Annotations
date: 2019-07-17 09:52:52
tags: github
---
## 反射，注解解析
目标：实现JUnit4的原型，执行某个类中标注了 @Test 注解的方法。
反射： Java提供的动态执行API
可以动态解析对象的类型，动态解析类的结构（属性、方法、构造器等）

案例1，动态检查对象的类型和其类的内部结构
```java
public class Demo03 {
    public static void main(String[] args) {
        test("123");
        test(123);
        test(123.0);
    }

    public static void test(Object obj) {
        //obj变量的类型是Object，obj引用的对象是什么类型？
        //在程序中检查 obj 引用的对象的类型？
        //obj.getClass() 是java在Object类型上定义的
        //方法， 可以动态获取当前对象的类型！
        Class cls = obj.getClass();
        System.out.println(cls); 

        //Class 类型上提供检查类的内部结构方法
        //Field: 字段，就是类中声明的成员变量
        //Declared: 声明的 
        // getDeclaredFields 获取cls类型的上声明的
        // 全部成员变量，包含静态变量和实例变量
        Field[] fields = cls.getDeclaredFields();
        for (Field field : fields) {
            System.out.println(field); 
        }
        // Method: 方法
        // getDeclaredMethods 返回cls类的定义的全部方法
        Method[] methods = cls.getDeclaredMethods();
        for (Method method : methods) {
            System.out.println(method);
        }
        //cls.getDeclaredConstructors() 全部构造器
        System.out.println("------------"); 
        //
    }
}
```

案例2，利用反射API 动态加载类，动态创建对象：
```java
public class Demo04 {
    public static void main(String[] args)
        throws Exception {
        /**
         * 动态加载类，动态创建对象
         * 运行期间才加载类，创建对象
         */
        Scanner in = new Scanner(System.in);
        System.out.print("输入类名：");
        String className = in.nextLine();
        //根据类名className动态加载类
        //如果类名错误，会出现类没有找到异常
        Class cls = Class.forName(className);
        System.out.println(cls);
        //动态创建 cls 对应的对象，cls必须包含无参数构造器
        //如果没有无参数构造器，则抛出 无此方法异常
        Object obj = cls.newInstance();
        System.out.println(obj); 
    }
}
```

案例3， 利用反射API找到方法上标注的注解：
```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

// Retention 用于声明注解 @Test 的保留范围
@Retention(RetentionPolicy.RUNTIME)
//Target 用于声明 注解的标注位置，当设置为METHOD以后
// 注解 @Test 就只能在方法上标注使用 
@Target(ElementType.METHOD) 
public @interface Test {

}

public class TestCase {

    public void t() {
        System.out.println("t");
    }

    @Test //自定义注解
    public void testHello() {
        System.out.println("Hello World!");
    }

    @Test
    public void demo() {
        System.out.println("demo!");
    }
}


public class Demo05 {
    public static void main(String[] args) throws Exception {
        /**
         * 利用反射API找到方法上标注的注解
         */
        String className = "day12.TestCase";
        //动态加载类
        Class cls=Class.forName(className);
        //动态获得类上声明的全部方法
        Method [] methods=cls.getDeclaredMethods();
        for (Method method : methods) {
            System.out.println(method);
            //Method 的API，可以检测方法上标注的注解
            Annotation[] anns=method.getAnnotations();
            for (Annotation ann : anns) {
                System.out.println(ann); 
            }
        }
    }
}
```

案例4，利用反射API动态执行方法
```java
    public class Demo06 {
        public static void main(String[] args)throws Exception{
            /**
             * 利用反射API动态执行方法
             * 根据动态输入的类名和方法名， 利用反射API
             * 加载类，找到方法，创建对象，调用方法。
             */
            //动态输入类名和方法名
            Scanner in = new Scanner(System.in);
            System.out.print("输入类名：");
            String className = in.nextLine();
            System.out.print("输入方法名:");
            String methodName=in.nextLine();
            //加载类
            Class cls = Class.forName(className);
            //利用API在cls上找到要调用的方法
            //getDeclaredMethod 在类上根据methodName寻找
            //一个 Method 对象，如果找不到抛出异常
            Method method=
              cls.getDeclaredMethod(methodName);
            //动态创建一个cls的对象
            Object obj = cls.newInstance();
            //执行方法, invoke 就是执行方法，但是必须提供
            //包含方法的对象, 如果对象上不包含方法则抛出异常
            Object val = method.invoke(obj);
            System.out.println(val); 
        }
    }
```

案例5， JUnit4 原型
```java
public class JUnit4Demo07 {
    public static void main(String[] args)throws Exception{
        /**
         * JUnit4 的原型: 执行某个类中标注了 @Test 注解的方法。
         */
        Scanner in = new Scanner(System.in);
        System.out.print("输入测试案例类名:");
        String className = in.nextLine();
        Class cls = Class.forName(className);
        Method[] methods = cls.getDeclaredMethods();
        Object obj = cls.newInstance();
        for (Method method : methods) {
            //查找方法上的某个注解getAnnotation(注解类)
            //如果找到指定注解，则返回一个注解对象
            //如果找不到指定注解，则返回null
            Annotation ann=method.getAnnotation(Test.class);
            if(ann==null) continue;
            method.invoke(obj);
        }
    }
}
```

### 解析控制器上的注解
加载控制器类，读取每个标注了@RequestMapping的方法，并且读取注解上标注的URL
案例:
1. 声明注解
```java
   @Retention(RUNTIME)
   @Target(METHOD)
   /** 
    * 标注在控制器方法上
    * 用途：将请求url地址映射到当前的方法上
    * 英文翻译： Request 请求  Mapping 映射
    */
   public @interface RequestMapping {
       //为注解定义参数
       public String value();
   }
```

2. 在控制器上标注注解:
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
       /**
        * 负责处理 /test/list.do 显示员工列表
        * 在Web程序运行期间， 将/test/list.do请求映射到
        * list() 方法上，也就是请求/test/list.do时候，
        * 执行list()方法
        */
       @RequestMapping("/test/list.do")
       public String list(HttpServletRequest request) {
           return "list";
       }
       /**
        * 负责处理 /test/add.do 显示添加员工界面
        */
       @RequestMapping("/test/add.do")
       public String add(HttpServletRequest request) {
           return "add";
       }
       // ...
   }
```

3. 利用Java反射API解析 @RequestMapping注解
```java
   public class Demo01 {
       public static void main(String[] args) throws Exception{
           /*
            * 解析 BizController类的@RequestMapping注解
            */
           String className="mvc.BizController";
           //动态加载类
           Class cls = Class.forName(className);
           //动态找到类的上全部方法
           Method[] methods=cls.getDeclaredMethods();
           //遍历全部方法，查找标注的 @RequestMapping 注解
           for(Method method:methods) {
               //利用RequestMapping.class类型找到的对象
               //是 RequestMapping 类型的对象，包含value方法
               RequestMapping ann=method.getAnnotation(
                       RequestMapping.class);
               if(ann!=null) {
                   //调用注解对象上的 value方法就能取到标注的值
                   String val = ann.value();
                   System.out.println(val); 
                   System.out.println(method);
               }
           }
       }
   }
```

## 解析控制器注解存储到Map
控制器RequestMapping注解的目的是将用户的请求映射到对应的业务方法：

#### RequestHandler
1. Map 每次只能存储一对 key-value
   1. key 是请求路径
   2. value 包含 方法 和 包含方法对象
2. 利用 RequestHandler 将包含方法的控制器对象和控制器方法封装为一个value，这样就可以将 控制器对象和控制器方法一起作为Value的存储到 map 对象中。
3. 利用 map 存储 请求路径（key） 和 RequestHandler（控制器对象和方法），这样在根据请求路径查找对应的控制器和方法时候会很快找到。

案例代码：
```java
/**
 * Handler 处理器
 * 请求处理器，用处理用户请求
 * 封装控制器对象和控制器上的业务方法 
 */
public class RequestHandler {
    private Object controller;
    private Method method;

    public RequestHandler() {
    }

    public RequestHandler(Object controller, Method method) {
        super();
        this.controller = controller;
        this.method = method;
    }

    public Object getController() {
        return controller;
    }

    public Method getMethod() {
        return method;
    }

    @Override
    public String toString() {
        return "RequestHandler [controller=" + controller + ", method=" + method + "]";
    }

}
```

#### HandlerMapping
用于解析 控制器 类将请求URL和控制器方法的对应关系缓存到Map中，并且提供根据url找到对应控制器方法，并且执行控制方法的功能
案例代码：
```java
public class HandlerMapping {
    /**
     * map 用于映射 请求 path 到对应的请求处理器
     * 如：将 /emp/list.do 映射到 bizColtroller.list()方法
     */
    private Map<String, RequestHandler> map=new HashMap<>();

    /**
     * 用于根据 path 执行对应控制器方法
     * @param path 请求路径
     * @param request 执行控制器时候的参数
     * @return 控制器执行以后的返回值，一般是目标JSP页面
     */
    public String execute(String path, HttpServletRequest request) throws Exception {
        RequestHandler handler = get(path);
        Method method = handler.getMethod();
        Object controller = handler.getController();
        Object val = method.invoke(
                controller, request);
        String value = (String)val;
        return value;
    }

    public RequestHandler get(String path) {
        return map.get(path);
    }

    /**
     * 初始化方法：解析控制器中的注解，将注解和注解
     * 标注的方法添加到 map中
     * @param className 控制器类名
     */
    public void init(String className) throws Exception{
        Class cls = Class.forName(className);
        Method[] methods = cls.getDeclaredMethods();
        Object controller = cls.newInstance();
        for(Method method:methods) {
            RequestMapping ann = method.getAnnotation(RequestMapping.class);
            if(ann!=null) {
                //找到注解上标注的路径
                String path=ann.value();
                //创建请求处理器对象，封装控制器对象和方法
                RequestHandler handler=
                        new RequestHandler(
                        controller, method);
                //请求路径和对应的“请求处理器”添加到map
                map.put(path, handler);
                System.out.println(path+":"+handler); 
            }
        }
    }
}
```

测试案例：
```java
public class Demo02 {
    public static void main(String[] args) throws Exception {
        /**
         * 测试 HandlerMapping的初始化方法是否可用
         */
        HandlerMapping mapping = new HandlerMapping();
        mapping.init("mvc.BizController"); 

        //执行 /test/add.do 对应的方法
        String val = mapping.execute("/test/add.do", null);
        System.out.println(val);     
    }
}
```

#### 增加 ContextConfigListener 加载配置文件
将配置控制器类名登记到XML文件中，这样可以灵活配置控制器的类名，以及控制器数量等
1. 编写配置文件 resources/beans.xml：
```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans>
       <bean class="mvc.BizController"></bean>
   </beans>
```

2. ContextConfigListener
```java
   /**
    * 初始化 URL 到 控制器 的映射表 
    */
  public class ContextConfigListener implements ServletContextListener {
       /**
        * 读取 beans.xml 配置文件，
        * 解析配置文件中定义的控制器类
        * 将控制器类的url映射到对应的控制器方法
        * @param xml
        * @return 包含url到控制器方法映射关系的
        *         HandlerMapping对象
        */
       public HandlerMapping loadControlers(String xml)
           throws Exception {
           SAXReader reader = new SAXReader();
           InputStream in = ContextConfigListener.class
               .getClassLoader().getResourceAsStream(xml);
           System.out.println(in); 
           Document doc = reader.read(in);
           in.close();
           Element root = doc.getRootElement();//beans
           List<Element> list=root.elements("bean"); 
           HandlerMapping mapping = new HandlerMapping();
           for(Element e:list) {
               //在bean元素上获取class属性的值作为类名
               String className=e.attributeValue("class");
               System.out.println(className);
               //利用类名初始化 HandlerMapping 中的map
               mapping.init(className);
           }
           return mapping;
       }
   
       public void contextInitialized(
               ServletContextEvent e)  {
           try {
               ServletContext ctx = e.getServletContext();
               //HandlerMapping mapping=new HandlerMapping();
               //mapping.init("mvc.BizController"); 
               HandlerMapping mapping=loadControlers("beans.xml");
   
               String path = ctx.getContextPath();
               ctx.setAttribute("root", path); 
   
               ctx.setAttribute("handlerMapping", mapping);
               System.out.println("初始化了handlerMapping"); 
           }catch(Exception ex) {
               ex.printStackTrace();
               throw new RuntimeException(ex);
           }
       }
       public void contextDestroyed(ServletContextEvent arg0)  { 
       }
   
   }
```

3. 测试：
```java
   public class Demo04 {
       public static void main(String[] args)throws Exception {
           /**
            * 测试: 读取配置文件，初始化 HandlerMapping
            */
           ContextConfigListener l = new ContextConfigListener();
           HandlerMapping mapping=l.loadControlers("beans.xml");
       }
   }
```

4. 配置监听器：
```xml
     <listener>
       <listener-class>mvc.ContextConfigListener</listener-class>
     </listener>
```

### 重构DispatcherServlet
根据用户的URL请求执行对应的 控制器，处理软件功能：
```java
/**
 * 单一前端控制器
 * 负责接收请求，处理与HTTP协议有关逻辑
 * 同时处理 get 和 post请求
 * 为了增加广泛的实用性， 可以处理任何的*.do 请求
 * 将 请求URL设置为 *.do
 */
public class DispatcherServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        try {
            //创建 HandlerMapping 对象 
            //找到对应的业务方法，执行业务方法
            //HandlerMapping mapping = new HandlerMapping();
            //mapping.init("mvc.BizController"); 

            //从ServletContext获取已经创建并初始化完成的
            //HandlerMapping 对象，这样可以避免每次都初始化
            //可以提高软件的性能
            HandlerMapping mapping=(HandlerMapping)
                getServletContext().getAttribute("handlerMapping");

            //获取用户发起的请求
            String pth = request.getServletPath();
            System.out.println(pth); 

            //处理编码问题
            request.setCharacterEncoding("UTF-8");

            //执行 URL 路径对应的业务方法
            String target=mapping.execute(pth, request);
            //target代表需要显示是目标网页，
            //约定target是以 redirect:为前缀
            //则进行重定向，如果控制器返回的字符串
            //以 redirect: 为开头，则重定向到 
            //redirect: 以后的URL地址
            if(target.startsWith("redirect:")) {
                String path = target.substring(9);
                response.sendRedirect(path); 
            }else {
                String path = "/WEB-INF/jsp/"+target+".jsp";
                request.getRequestDispatcher(path).forward(request, response); 
            }
        }catch(Exception e) {
            e.printStackTrace();
            //抛出一个 ServletException，这个异常是
            //作用是，将异常e抛给Web容器，Web容器会
            //显示 500 错误页面到浏览器
            throw new ServletException(e);
        }
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

}
```

配置：
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

添加控制器方法并且测试：
```java
    @RequestMapping("/test/hello.do")
    public String hello(HttpServletRequest request) {
        request.setAttribute("msg", "HI"); 
        return "hello";
    }
```