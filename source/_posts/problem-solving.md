---
title: problem-solving
date: 2019-07-20 15:39:59
tags: github
---
## 常见问题

### Tomcat启动失败的解决方案
首先，如果启动时，弹出对话框，对话框中提示了端口号，多是因为端口号冲突，例如此前已经启动了Tomcat却没有关闭，仍再次启动，就会出现冲突。如果要解决问题，可以先停止此前占用端口的程序，例如在Eclipse找到停止按钮，或者在Tomcat的bin目录下执行shutdown指令，如果不知道如何，可以重启电脑！

如果启动过程中报告异常信息，提示`ClassNotFoundException`或者`FileNotFoundException`，就检查对应的类是否存在，如果提示的类是依赖的jar包中的类，可能是jar包文件已经损坏，需要重新下载(可以删除.m2中的jar包或者更换版本)。

如果启动过程中报告的异常信息是`ZipException`，一定是某个jar包损坏，则检查最近添加的依赖的jar包并重新下载。

如果启动过程中报告的异常信息是`LifeCycleException`，通常是由于缓存问题导致的，则需要Clean项目及Tomcat。

### 关于GET和POST的区别

GET请求会将请求参数体现在URL中，POST请求会将请求参数封装在请求体中，并不会体现在URL中。

GET请求不适用于涉及隐私、安全方面的数据，也不适用于传输数据量较大的数据，通常限制值是2K，该值既取决于客户端的浏览器，也取决于服务器端。

所以，在涉及隐私、安全的数据提交，或者较大数据的提交（特别是上传文件），都应该优先考虑POST方式提交请求。

由于POST方法将请求参数封装在请求体中，没有体现在URL中，所以，如果涉及URL分享等操作，必须使用GET方式提交请求。

在发生请求时，如果使用GET请求，将一次性将请求的URL提交到服务器，所以，请求参数也就直接提交到了服务器，如果使用POST请求，会先向服务器提交第1次请求，此次请求并不携带请求参数，当服务器响应100后，客户端发出第2次请求，再将请求参数提交到服务器。所以，GET请求的访问速度比POST请求更快。

### 关于转发和重定向

无论是转发还是重定向，都是客户端请求的第1个目标无法实现请求的响应，需要配合服务器端的其它组件来完成响应！

转发的原因是因为使用控制器可以处理用户请求，但是，当得到数据结果后，存在不便于显示的问题，毕竟控制器是Java类，不便于组织HTML代码，所以，会将数据结果转发给JSP页面，由JSP页面来完成数据的呈现，当然，JSP也存在不便于处理数据逻辑的问题，即与HTML高度相似的代码结构中不便于编写Java代码，所以，推荐的做法就是控制器负责处理数据，得到数据结果后把这些数据结果转发给JSP，由JSP呈现，整个过程是发生在服务器内部的，对于客户端来说是不可见的，所以，在转发时，客户端只发出了1次请求，请求的URL就是控制器的URL，并且，即使WEB-INF文件夹的内容是不可以被客户端直接访问的，也不影响把JSP放在这个文件夹下。

重定向是客户端发出第1次请求后，服务器端无法完成最终的响应，所以，只能给出302(通常是302)响应码，让客户端请求另一个URL来完成最终响应，在整个过程中，客户端是发出了2次请求的，同时，客户端也明确第2次的请求目标，所以，在客户端的浏览器中，会显示第2次请求的URL，由于客户端共发出2次请求，所以，在没有经过特殊的处理方式时，第1次请求的数据不可以直接应用到第2次请求中。