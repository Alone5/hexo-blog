---
title: AJAX
date: 2019-07-27 22:16:29
tags: github
---
## AJAX

### 简介
**AJAX**即“**Asynchronous JavaScript and XML**”（异步的 JavaScript 与 XML 技术），指的是一套综合了多项技术的浏览器端网页开发技术。Ajax的概念由杰西·詹姆士·贾瑞特所提出。

传统的Web应用允许用户端填写表单（form），当提交表单时就向网页服务器发送一个请求。服务器接收并处理传来的表单，然后送回一个新的网页，但这个做法浪费了许多带宽，因为在前后两个页面中的大部分 HTML 码往往是相同的。由于每次应用的沟通都需要向服务器发送请求，应用的回应时间依赖于服务器的回应时间。这导致了用户界面的回应比本机应用慢得多。

与此不同，AJAX应用可以仅向服务器发送并取回必须的数据，并在客户端采用JavaScript处理来自服务器的回应。因为在服务器和浏览器之间交换的数据大量减少，服务器回应更快了。同时，很多的处理工作可以在发出请求的客户端机器上完成，因此Web服务器的负荷也减少了。

类似于DHTML，AJAX不是指一种单一的技术，而是有机地利用了一系列相关的技术。虽然其名称包含XML，但实际上数据格式可以由JSON代替，进一步减少数据量，形成所谓的AJAJ。而客户端与服务器也并不需要异步。一些基于AJAX的“派生／合成”式（derivative/composite）的技术也正在出现，如AFLAX。

### 1. 服务器端如何响应请求

传统的响应方式有转发和重定向，这样的做法有很多问题，比如：转发和重定向都决定了响应的具体页面，不适合多种客户端（浏览器、Android手机、Android平板电脑、iOS手机、iOS平板电脑……）的项目，因为不同的终端设备的性能特征是不一样的，把同样的一个页面都显示给不同的终端设备是极不合适的！正确的做法应该是“服务器端只响应客户端所需要的数据”，至于这些数据如何呈现在终端设备中，由各客户端的开发团队去解决！

如果使用响应正文的方式，还存在“响应数据量小”的优势，则响应速度更快，产生的流量消耗小，用户体验好！

### 2. 服务器端响应正文

假设客户端会提交`http://localhost:8080/AJAX/user/login.do`请求，如果需要响应方式是“响应正文”，则需要在处理请求的方法之前补充添加`@ResponseBody`注解：
```java
    @Controller
    @RequestMapping("user")
    public class UserController {

        @RequestMapping("login.do")
        @ResponseBody
        public String login() {
            return "LOGIN SUCCESS.";
        }
    }
```

>默认情况下，响应的内容使用了ISO-8859-1编码，所以，不支持中文。

### 3. 服务器响应的正文格式--JSON格式

JSON(JavaScript Object Notation, JS 对象简谱) 是一种轻量级的数据交换格式。它基于 ECMAScript (欧洲计算机协会制定的js规范)的一个子集，采用完全独立于编程语言的文本格式来存储和表示数据。简洁和清晰的层次结构使得 JSON成为理想的数据交换语言。易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。

通常，服务器向客户端响应的数据可能不只是1个数据，以登录操作为例，也许可以响应为`1`表示登录成功，使用`2`表示登录失败且是因为用户名错误，使用`3`表示密码错误，则客户端就可以通过对这个值的判断，得知当前操作结果，但是，其它操作可能会需要更多的数据，例如“客户端尝试获取当前登录的用户信息”，需要响应的数据可能包括：用户名、手机号码、电子邮箱、年龄等一系列数据，由于响应结果只是1个字符串，要把这些数据很好的组织起来，才可以方便客户端从这1个字符串中获取其中某部分的数据，否则，如果只是响应为`"root13800138001root@163.com26"`这样，客户端就无法处理这个响应结果。

早期通常使用XML语法来组织这些数据：

```xml
    <user>
        <username>root</username>
        <age>26</age>
        <phone>13800138001</phone>
        <email>root@163.com</email>
    </user>
```

使用XML存在的问题：

- 数据量略大，传输略慢，流量消耗略大，用户体验略差；
- 解析难度大。

目前推荐使用的组织数据的格式是JSON格式，以上数据使用JSON组织后的表现为：

```json
    {
        "username":"root",
        "age":26,
        "phone":"13800138001",
        "email":"root@163.com",
        "skill":["Java", "Java OOP", "Java SE", "Java WEB", "MySQL", "Spring"],
        "department":{
            "id":1,
            "name":"RD"
        }
    }
```

JSON数据在Javascript中是默认即识别的对象，可以直接得到其中的属性值：

```html
    <script type="text/javascript">
        var json = {
            "username":"root",
            "age":26,
            "phone":"13800138001",
            "email":"root@163.com"
        };

        console.log(json.username);
        console.log(json.age);
    </script>
```

关于JSON数据格式：

- 使用`{}`表示对象，整个JSON数据就是1个对象；
- 所有的属性名都是字符串类型的，在JavaScript中，可以使用单引号或者双引号框柱，因为JSON数据可能在多种不同的编程语言中都出现，一般推荐使用双引号，属性值如果是字符串，也需要使用双引号框柱，如果是数值或者布尔值，则可以不用双引号框柱，属性名和属性值使用冒号`:`分隔，多个属性的配置之间使用逗号`,`分隔；
- 属性值的类型还可以是数组，使用中括号`[]`框柱数组的各元素，各元素之间使用逗号`,`分隔，在JavaScript中处理时，使用例如`json.skill`就可以获取到整个数组，使用`json.skill.length`就可以获取数组的长度，使用`json.skill[0]`就可以获取数组中下标为0的元素，也可以使用循环语法进行循环；
- 属性值的类型还可以是另一个对象，使用`{}`表示对象。

如果在JavaScript中，得到是一个使用JSON语法组织的字符串，而不是JSON对象，可以调用`JSON.parse(str)`函数，将字符串转换为JSON对象。

### 4. 服务器端实现响应JSON格式的数据

如果需要服务端响应JSON格式的数据，不可能自行拼接出JSON格式的字符串，可以通过工具来解决
1. 需要添加`jackson-databind`的依赖：

```xml
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.9.8</version>
    </dependency>
```

2. 在项目中自定义`cn.tedu.ajax.JsonResult`响应结果类型

3. 并修改处理请求的方法，返回值类型使用以上自定义的类型：

```java
    @RequestMapping("login.do")
    @ResponseBody
    public JsonResult login(
        @RequestParam("username") String username,
        @RequestParam("password") String password) {
        Integer state;
        String message = null;

        if ("root".equals(username)) {
            if ("1234".equals(password)) {
                state = 1;
            } else {
                state = 3;
                message = "密码错误！";
            }
        } else {
            state = 2;
            message = "用户名不存在！";
        }

        JsonResult jsonResult = new JsonResult();
        jsonResult.setState(state);
        jsonResult.setMessage(message);
        return jsonResult;
    }
```

4. 在这里如果直接运行，会提示406错误，需要在spring.xml中添加配置：

```xml
    <!-- 注解驱动 -->
    <mvc:annotation-driven />
```

然后，控制器中处理请求的方法响应的正文就是JSON格式的字符串。

在控制器中响应正文时，需要添加`@ResponseBody`注解，SpringMVC框架内置了一系列的转换器(Converter)，用于将方法的返回值转换为响应的正文，在这一系列的转换器中，SpringMVC设计了对应关系和优先级，例如，当方法的返回值类型是`String`时，就会自动调用`StringHttpMessageConverter`，当项目中添加了`jackson-databind`依赖时，如果方法的返回值类型是SpringMVC默认不识别的，就会自动使用`Jackson`依赖中的转换器！`Jackson`依赖还会将响应头(Response Headers)中的`Content-Type`设置为`application/json, charset=utf-8`。

**小结：需要自定义数据类型，以决定响应的JSON数据格式(有哪些属性，分别是什么类型)，然后用自定义类型作为方法的返回值，并处理完成后返回该类型的对象，Jackson依赖就会自动的设置为支持中文，且把响应的对象转换成JSON字符串。**

### 5. jQuery - AJAX

在实际实现时，通常是基于jQuery框架实现AJAX访问，主要是因为原生技术的代码比较繁琐，且存在浏览器的兼容性问题，在jQuery中，定义了`$.ajax()`函数，用于处理AJAX请求，调用该函数即可实现异步访问：

```html
    <script type="text/javascript" src="jquery-3.4.1.min.js"></script>
    <script type="text/javascript">
    $("#btn-login").click(function(){
        // $.ajax()函数的参数是1个JSON对象
        // url：请求提交到哪里
        // data：需要提交的请求参数
        // type：请求类型
        // dataType：服务器端响应的数据类型，可以是text/xml/json，取值取决于Response Headers中的Content-Type
        // success：服务器端HTTP响应码是2xx时的回调(callback)函数，函数的参数就是服务器端响应的正文结果
        $.ajax({
            "url":"user/login.do",
            "data":$("#form-login").serialize(),
            "type":"post",
            "dataType":"json",
            "success":function(result) {
                if (result.state == 1) {
                    alert("登录成功！");
                } else {
                    alert(result.message);
                }
            }
        });
    });
    </script>
```