---
title: Spring-Request
date: 2019-07-22 20:19:19
tags: github
---
## 1、接收请求参数

#### 1、【不推荐】通过HttpServletRequest获取请求参数
可以在处理请求的方法中，添加`HttpServletRequest`类型的参数，在处理过程中，调用该参数对象的`String getParameter(String name)`就可以获取请求参数。

不推荐使用这种做法！主要原因有：
- 使用比较繁琐，代码量较多；
- 需要手动实现数据类型的转换；
- 不便于实现单元测试。

#### 2、【推荐】将请求参数设计为处理请求的方法的参数

假设客户端将提交名为`username`的参数，则在控制器的方法中添加同名参数即可，参数的类型可以是期望的数据类型，例如`username`是`String`类型的，则声明为`String username`即可：
```java
    @RequestMapping("handle_reg.do")
    public String handleReg(String username, String password) {
        System.out.println("UserController.handleReg()");
        System.out.println("\t[2]username=" + username);
        System.out.println("\t[2]password=" + password);
        return null;
    }
```

- 使用这种做法时，需要保证参数的名称与客户端提交的请求参数名称保持一致，否则，在控制器中的参数将是`null`值。
- 使用这种做法的优点很多，基本上解决了使用`HttpServletRequest`获取参数时的所有问题！
- 使用这种做法并不适用于请求参数的数量较多的应用场景！

#### 3、【推荐】使用封装的类型接收较多的请求参数

当客户端提交的请求参数较多时，可以将这些参数全部封装为1个自定义的数据类型，例如：
```java
    public class User {
        private String username;
        private String password;
        private Integer age;
        private String phone;
        private String email;

        //生成GET、SET方法和toString
    }
```

将该类型作为处理请求的方法的参数即可：
```java
    @RequestMapping("handle_reg.do")
    public String handleReg(User user) {
        System.out.println("UserController.handleReg()");
        System.out.println("\t" + user);
        return null;
    }
```
>在使用这种做法时，需要保证客户端提交的请求参数，与自定义的数据类型中的属性名称是保持一致的！

#### 4、小结
第1种使用`HttpServletRequest`的方式是不会再使用的。

如果请求参数的数量较少，且从业务功能来说参数的数量基本固定，推荐使用第2种方式，即直接将请求参数逐一的设计在处理请求的方法中，否则，就使用第3种方式，将多个参数封装成1个自定义的数据类型。

另外，第2种方式和第3种方式是可以组合使用的，即出现在同一个处理请求的方法中！

## 2、重定向

当处理请求的方法的返回值是`String`类型的，则返回的字符串使用`redirect:`作为前缀，加上重定向的目标路径，就可以实现重定向的效果。

假设注册一定成功，且注册成功后需要跳转到登录页面，则：
```java
    @RequestMapping("handle_reg.do")
    public String handleReg(User user) {
        System.out.println("UserController.handleReg()");
        System.out.println("\t" + user);

        // 注册成功，重定向到登录页
        // 当前位置：handle_reg.do
        // 目标位置：login.do
        return "redirect:login.do";
    }
```

## 3、转发

#### 1、执行转发

当处理请求的方法的返回值是`String`类型，默认情况下，返回值就表示转发的意思，返回值将经过视图解析器，确定转发到的目标页面。

转发时，处理请求的方法的返回值也可以使用`forward:`作为前缀，由于默认就是转发，所以不必显式的添加前缀。

#### 2、【不推荐】通过HttpServletRequest封装需要转发的数据

假设在登录过程中，仅当用户名为`root`且密码是`1234`时允许登录，否则，在错误提示页面中提示错误的原因。

由于错误信息可能有2种，分别是用户名错误和密码错误，使用JSP页面结合EL表达式可以显示转发的数据，在控制器转发之前，就需要将错误信息封装到`HttpServletRequest`对象中，则后续JSP页面才可以通过EL表达式读取`HttpServletRequest`对象中的数据。

可以在处理请求的方法的参数列表中添加`HttpServletRequest`类型的参数，当添加了参数后，调用`HttpServletRequest`参数对象的`setAttribute(String name, String value)`方法封装需要转发的数据即可，无需获取转发器对象执行转发，只要最后返回字符串就表示转发：
```java
    @RequestMapping("handle_login.do")
    public String handleLogin(String username, String password,
            HttpServletRequest request) {

        // 判断用户名是否正确
        if ("root".equals(username)) {
            // 是：判断密码是否正确
            if ("1234".equals(password)) {
                // 是：登录成功，重定向到主页
                return "redirect:index.do";
            } else {
                // 否：密码错误
                request.setAttribute("errorMessage", "密码错误");
                return "error";
            }
        } else {
            // 否：用户名错误
            request.setAttribute("errorMessage", "用户名错误");
            return "error";
        }
    }
```
>这种做法依然是不推荐的，使用了`HttpServletRequest`作为参数后不便于执行单元测试。

#### 3、【更不推荐】使用ModelAndView

使用`ModelAndView`作为处理请求的方法的返回值类型，在返回结果之前，调用`ModelAndView`对象的`setViewName(String viewName)`方法确定转发的视图名称，调用`addObject(String name, Object value)`方法封装需要转发的数据，然后返回结果即可：
```java
@RequestMapping("handle_login.do")
public ModelAndView handleLogin(String username, String password) {
    // 创建返回值对象
    ModelAndView mav = new ModelAndView();
    // 判断用户名是否正确
    if ("root".equals(username)) {
        // 是：判断密码是否正确
        if ("1234".equals(password)) {
            // 是：登录成功，重定向到主页
            return null;
        } else {
            // 否：密码错误
            mav.addObject("errorMessage", "ModelAndView：密码错误");
            mav.setViewName("error");
            return mav;
        }
    } else {
        // 否：用户名错误
        mav.addObject("errorMessage", "ModelAndView：用户名错误");
        mav.setViewName("error");
        return mav;
    }
}
```
>因为对于初学SpringMVC的人来说，`ModelAndView`是一个新的、比较麻烦的数据类型，并且SpringMVC提供了更简单的操作方式，所以不推荐使用`ModelAndView`。

#### 4、【推荐】使用ModelMap封装需要转发的数据

使用`ModelMap`的方式与使用`HttpServletRequest`几乎完全相同：
```java
    @RequestMapping("handle_login.do")
    public String handleLogin(String username, String password,
            ModelMap modelMap) {

        // 判断用户名是否正确
        if ("root".equals(username)) {
            // 是：判断密码是否正确
            if ("1234".equals(password)) {
                // 是：登录成功，重定向到主页
                return "redirect:index.do";
            } else {
                // 否：密码错误
                modelMap.addAttribute("errorMessage", "ModelMap：密码错误");
                return "error";
            }
        } else {
            // 否：用户名错误
            modelMap.addAttribute("errorMessage", "ModelMap：用户名错误");
            return "error";
        }
    }
```

相比`HttpServletRequest`而言，使用`ModelMap`更加易于实现单元测试，并且更加轻量级，所以，推荐使用这种方式来封装需要转发的数据。

练习：假设`root`是已经被注册的用户名，在处理注册时，如果用户提交的用户名是`root`，则提示错误，否则，视为注册成功，重定向到登录页。

## 4、@RequestMapping注解

#### 4.1. 基本使用

在处理请求的方法之前添加`@RequestMapping`，可以配置请求路径与处理请求的方法的映射关系。

除此以外，还可以在控制器类之前添加该注解，表示增加了访问路径中的层级，例如：
```java
    @Controller 
    @RequestMapping("user")
    public class UserController {
    }
```

添加该注解以后，原本通过`login.do`访问的请求路径就需要调整为`user/login.do`才可以访问。通常，推荐为每一个控制器类都添加该注解！同时在类和方法之前都添加了注解后，最终的访问路径就是类与方法的注解中的路径组合出来的URL。

>在配置路径时，会无视两端的`/`符号

#### 4.2. 注解配置

在配置`@RequestMapping`时，可以显式的配置为：
```java
	@RequestMapping(value="reg.do")
```

该属性的数据类型是`String[]`，也可以配置为：
```java
	@RequestMapping(value= {"reg.do", "register.do"})
```

则后续无论通过这里的哪个URL都会导致映射的方法被执行。

源代码中在`value`属性的声明上方还使用了`@AliasFor`注解，表示`value`和`path`是完全等效的！从SpringMVC 4.2版本开始支持使用`path`属性，并推荐使用`path`属性取代`value`属性。

在使用时，还可以指定`method`属性，该属性的作用是用于限制请求方式，例如：
```java
    @RequestMapping(path= {"reg.do", "register.do"}, method=RequestMethod.POST) 
```
>以上代码表示提交的请求必须是POST请求，如果不是，会导致405错误，在没有配置`method`之前，是不限定请求方式的，如果配置了，则必须使用配置的请求方式中的某一种！当为注解配置多个属性时，每一个属性都必须显式的指定属性名称！

## 5. 关于@RequestParam注解

可以在处理请求的方法的参数之前添加`@RequestParam`注解，首先，使用该注解可以解决名称不一致的问题，即客户端提交的请求参数名称与服务器端处理请求的方法的参数名称不一致的问题，例如：
```java
	@RequestParam("uname") String username
```
>如果添加了该注解，仍然存在名称不一致的问题，会导致400错误（如果没有添加该注解，即使名称不一致，服务器端的参数只是null值，并不会报错）

原因在于在该注解的源代码中：
```java
	boolean required() default true;
```

所以，添加了该注解，默认是必须提交指定名称的参数的！如果希望该请求参数不是必须提交的，可以：
```java
	@RequestParam(name="uname", required=false) String username 
```

该注解中还有：
```java
	String defaultValue() default ValueConstants.DEFAULT_NONE;
```

该属性用于指定默认值，即客户端没有提交指定名称的参数时，默认为某个值，例如：
```java
    @RequestParam(name="uname", required=false, defaultValue="admin") String username
```
>注意：在设置默认值时，必须显式的将`required`属性设置为`false`。

**小结：在什么时候需要使用该注解？**

- 前后端使用的名称不统一时；
- 强制要求客户端必须提交某参数时；
- 允许客户端不提交某参数，且视为提交了某默认值时；
- 其他固定使用场景。
