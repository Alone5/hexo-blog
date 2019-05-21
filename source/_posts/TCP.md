---
title: TCP通信
date: 2019-05-18 14:41:21
tags: github
---
### Socket 原理
#### Socket 简介
- **socket起源于Unix，而Unix/Linux基本哲学之一就是：“一切即文件”，都可以用"打开open -> 读写write/read -> 关闭close"模式来操作。**
- **简单来说socket就是该模式的一个实现，socket即是一种特殊的文件，一些socket函数就是对其进行的操作(读/写IO,关闭，打开)。**
- **我们在传输数据的时候，可以只使用（传输层）TCP/IP协议，但是那样的话，如果没有应用层，便无法识别数据内容，如果想要使传输的数据有意义，则必须使用到应用成协议，应用层协议有很多，比如HTTP,FTP,TELNET等，也可以自己定义应用层协议。WEB协议使用HTTP协议作应用层协议，以封装HTTP文本信息，然后使用TCP/IP协议作传输层协议将它发到网络上。**

#### Socket概念
- **Socket是一个针对TCP和UDP编程的接口，你可以借助它建立TCP/IP连接等等。而TCP和UDP协议属于传输层。而HTTP是个应用层的协议，它实际上也建立在 TCP协议之上.（HTTP是轿车，提供了封装或者是现实数据的具体形式；socket是发动机，提供了网络通信的能力)。**
- **socket称为"套接字",适用于网络通信的方法。socket是对tcp/IP协议的封装，socket本身并不是协议，而是一个调用接口(api)。通过socket我们才能使用TCP/IP协议。Socket的出现只是使得程序员更方便地使用TCP/IP协议栈而已，是对TCP/IP协议的抽象，从而形成了我们知道的一些最基本的函数接口。**

#### 获取本地地址和端口号
- **java.net.Socket为套接字类，通过Socke获取：**
	- **int getLocalPort() :**
		- **用获取本地使用的端口号**
	- **getLocalSocketAddress() **
		- **返回此套接字绑定的端点的地址，如果尚未绑定则返回 null。**
	- **IntAddress getLocalAddress() :**
		- **用于获取套接字绑定的本地地址**
- **使用InetAddress获取本地的地址方法:**
	- **String getCanonicalHostName() :**
		- **获取此IP地址的完全限定域名**
	- **String getHostAddress() :**
		- **返回IP地址字符串（以文本表现形式）**
	- **String getCanonicalHostName() :**
		- **获取此 IP 地址的完全限定域名。**
	- **String getHostName() :** 
		- **获取此 IP 地址的主机名。**

#### 获取远端地址和端口号
- **通过Socket获取：**
	- **int getPort() :**
		- **用于获取远端使用的端口号**
	- **InetAddress .getInetAddress() :**
		- **用于获取套接字绑定的远端地址**

#### 获取网络输入流和网络输出流
- **通过Socket获取输入流与输出流：**
	- **InputStream getInputStream() :**
		- **用于返回套接字的输入流**
	- **OutputStream getOutputStream() :**
		- **用于返回套接字的输出流**

- **close方法：当使用Socket进行通讯完毕后，要关闭Socket以释放系统资源。**
- **void close() ：**
	- **关闭此套接字。当关闭了该套接字后也会同时关闭由此获取的输入流与输出流**












