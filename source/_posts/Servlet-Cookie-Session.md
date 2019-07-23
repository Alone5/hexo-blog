---
title: Servlet-Cookie/Session
date: 2019-07-11 19:28:01
tags: github
---
## Cookie、Session
#### 简介
- 由于HTTP协议是无状态的协议，所以服务端需要记录用户的状态时，就需要用某种机制来识具体的用户，这个机制就是Session.典型的场景比如购物车，当你点击下单按钮时，由于HTTP协议无状态，所以并不知道是哪个用户操作的，所以服务端要为特定的用户创建了特定的Session，用用于标识这个用户，并且跟踪用户，这样才知道购物车里面有几本书。这个Session是保存在服务端的，有一个唯一标识。在服务端保存Session的方法很多，内存、数据库、文件都有。集群的时候也要考虑Session的转移，在大型的网站，一般会有专门的Session服务器集群，用来保存用户会话，这个时候 Session 信息都是放在内存的，使用一些缓存服务比如Memcached之类的来放 Session。

- **服务端如何识别特定的客户？**这个时候Cookie就登场了。每次HTTP请求的时候，客户端都会发送相应的Cookie信息到服务端。实际上大多数的应用都是用 Cookie 来实现Session跟踪的，第一次创建Session的时候，服务端会在HTTP协议中告诉客户端，需要在 Cookie 里面记录一个Session ID，以后每次请求把这个会话ID发送到服务器。客户端的浏览器禁用了 Cookie 怎么办？一般这种情况下，会使用一种URL重写的技术来进行会话跟踪，即每次HTTP交互，URL后面都会被附加上一个诸如 sid=xxxxx 这样的参数，服务端据此来识别用户。

- Cookie（复数形态：Cookies），是指某些网站为了辨别用户身份、进行session跟踪而储存在用户本地终端上的数据（通常经过加密）。
- Cookie是由服务端生成的，发送给客户端（通常是浏览器）的。Cookie总是保存在客户端中，按在客户端中的存储位置，可分为内存Cookie和硬盘Cookie：
- 内存Cookie由浏览器维护，保存在内存中，浏览器关闭后就消失了，其存在时间是短暂的。
- 硬盘Cookie保存在硬盘里，有一个过期时间，除非用户手工清理或到了过期时间，硬盘Cookie不会被删除，其存在时间是长期的。所以，按存在时间，可分为非持久Cookie和持久Cookie。
- 作用
	Cookie的根本作用就是在客户端存储用户访问网站的一些信息。典型的应用有：
  1. 记住密码，下次自动登录。
  2. 购物车功能。
  3. 记录用户浏览数据，进行商品（广告）推荐。

- 缺陷
  1. Cookie会被附加在每个HTTP请求中，所以无形中增加了流量。
  2. 由于在HTTP请求中的Cookie是明文传递的，所以安全性成问题。（除非用HTTPS）
  3. Cookie的大小限制在4KB左右。对于复杂的存储需求来说是不够用的。

**总结一下：**
- Session是在服务端保存的一个数据结构，用来跟踪用户的状态，这个数据可以保存在集群、数据库、文件中；
- Cookie是客户端保存用户信息的一种机制，用来记录用户的一些信息，也是实现Session的一种方式

### Cookie和Session的区别
1. 存放位置不同
Cookie保存在客户端，Session保存在服务端。
2. 存取方式的不同
Cookie中只能保管ASCII字符串，假如需求存取Unicode字符或者二进制数据，需求先进行编码。Cookie中也不能直接存取Java对象。若要存储略微复杂的信息，运用Cookie是比拟艰难的。 

而Session中能够存取任何类型的数据，包括而不限于String、Integer、List、Map等。Session中也能够直接保管Java Bean乃至任何Java类，对象等，运用起来十分便当。能够把Session看做是一个Java容器类。 

3. 安全性（隐私策略）的不同 
Cookie存储在浏览器中，对客户端是可见的，客户端的一些程序可能会窥探、复制以至修正Cookie中的内容。而Session存储在服务器上，对客户端是透明的，不存在敏感信息泄露的风险。 假如选用Cookie，比较好的方法是，敏感的信息如账号密码等尽量不要写到Cookie中。最好是像Google、Baidu那样将Cookie信息加密，提交到服务器后再进行解密，保证Cookie中的信息只要本人能读得懂。而假如选择Session就省事多了，反正是放在服务器上，Session里任何隐私都能够有效的保护。 
4. 有效期上的不同 
只需要设置Cookie的过期时间属性为一个很大很大的数字，Cookie就可以在浏览器保存很长时间。 由于Session依赖于名为JSESSIONID的Cookie，而Cookie JSESSIONID的过期时间默许为–1，只需关闭了浏览器（一次会话结束），该Session就会失效。
5. 对服务器造成的压力不同 
Session是保管在服务器端的，每个用户都会产生一个Session。假如并发访问的用户十分多，会产生十分多的Session，耗费大量的内存。而Cookie保管在客户端，不占用服务器资源。假如并发阅读的用户十分多，Cookie是很好的选择。
6. 跨域支持上的不同 
Cookie支持跨域名访问，例如将domain属性设置为“.baidu.com”，则以“.baidu.com”为后缀的一切域名均能够访问该Cookie。跨域名Cookie如今被普遍用在网络中。而Session则不会支持跨域名访问。Session仅在他所在的域名内有效。 

### HttpSession 会话机制    -->Servlet的会话机制的实现
创建于服务器端，保存于服务器，维护于服务器端,每创建一个新的Session,服务器端都会分配一个唯一的ID，并且把这个ID保存到客户端的Cookie中，保存形式是以JSESSIONID来保存的。

- 通过HttpServletRequest.getSession 进行获得HttpSession对象，通过setAttribute()给会话赋值，可以通过invalidate()将其失效。
- 每一个HttpSession有一个唯一的标识SessionID，只要同一次打开的浏览器通过request获取到session都是同一个。浏览器不同SessionID 也不同
- WEB容器默认的是用Cookie机制保存SessionID到客户端，并将此Cookie设置为关闭浏览器失效，Cookie名称为：JSESSIONID
- 每次请求通过读取Cookie中的SessionID获取相对应的Session会话
- HttpSession的数据保存在服务器端，所以不要保存数据量耗资源很大的数据资源，必要时可以将属性移除或者设置为失效
- HttpSession可以通过setMaxInactiveInterval()设置失效时间(秒)或者在web.xml中配置
```xml
   <session-config>
         <!--单位：分钟-->
       <session-timeout>30</session-timeout>
   </session-config>
```

- HttpSession默认使用Cookie进行保存SessionID，当客户端禁用了Cookie之后，可以通过URL重写的方式进行实现。
  - 可以通过response.encodeURL(url)  进行实现
  - API对encodeURL的结束为，当浏览器支持Cookie时，url不做任何处理；当浏览器不支持Cookie的时候，将会重写URL将SessionID拼接到访问地址后。

### Cookie的使用：
- 添加Cookie：
```java
  Cookie cookie = new Cookie("user", "suntao");
  cookie.setMaxAge(7*24*60*60);     // 一星期有效
  response.addCookie(cookie);
```

- 获取Cookie
```java
  // 因为取得的是整个网页作用域的Cookie的值，所以得到的是个数组
  Cookie[] cookies = request.getCookies();
  for(int i = 0 ; i < cookies.length ; i++){
		String name = cookies[i].getName() ;
		String value = cookies[i].getValue() ;
   } 
```

>cookie的存活期：默认为-1
会话Cookie：把Cookie保存到浏览器上，当存活期为负数
持久Cookie：把Cookie保存到文件中，当存活期为正数
设置存活期：c.setMaxAge();

**Cookie中的特殊字符和中文**
由于Cookie是在Http协议头部传送的， Http协议头必须是 ISO8859-1 编码，并且不能使用特殊字符和中文。
- 利用 Escape 编码进行解决，是HTTP协议标准。
- 数据加入Cookie之前进行编码即可。
- Java 提供了编码API
```
	URLEncoder.encode("您好Cookie", "UTF-8")
```

利用Java Servlet API 下发Cookie
- 下发Cookie，response 对象上有下发Cookies API：
```java
public class AddCookieServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;
    /**
     *  请求路径 /add-cookie
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 利用response下发一个Cookie到浏览器
        //1. 创建一个Cookie
        Cookie cookie = new Cookie("message", URLEncoder.encode("Hello World!","UTF-8"));
        Cookie cookie2 = new Cookie("test", URLEncoder.encode("您好Cookie", "UTF-8"));

        //2. 下发Cookie
        response.addCookie(cookie);
        response.addCookie(cookie2);
        response.setContentType("text/html; charset=UTF-8");
        response.getWriter().print("OK"); 
    }
}
```

### 什么时候需要使用Session

通常，使用Session存储：

1. 用户的身份的唯一标识，例如：用户的id；
2. 使用频率较高的数据，例如：用户名；
3. 不便于使用其它解决方案去存储或者传递的数据。