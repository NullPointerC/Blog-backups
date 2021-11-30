---
title: JavaWeb-协议和服务器
categories: [Java]
tags:
  - web
  - Java
  - backend
date: 2021-07-30 14:40:53
---
## 一、HTTP协议

### 1.1 客户端与服务端的交互原理

![image-20210730150647023](https://gitee.com/cao_ziqiang/img/raw/master/20210730150647.png)

客户端的浏览器版本很多，为了实现不同版本的浏览器与服务器的通信，就需要约定好一个<font color='red'>统一的协议</font>，双方按照协议交互数据，这样就能向下屏蔽底层差异。

![image-20210730151443537](https://gitee.com/cao_ziqiang/img/raw/master/20210730151443.png)

### 1.2 HTTP协议

HTTP：超文本传输协议(Hyper Text Transfer Protocol)

作用：规范了浏览器和服务器的数据交互

特点：

1、简单快速

2、灵活

3、无连接

4、无状态

5、支持B/S和C/S架构

<font color='red'>注意：HTTP1.1版本之后支持可持续连接</font>

### 1.3 HTTP协议的交互流程

由客户端发起建立连接的请求，再发送请求给服务端，服务端返回响应给客户端，最后关闭连接

![image-20210730151634186](https://gitee.com/cao_ziqiang/img/raw/master/20210730151634.png)

### 1.4 HTTP协议请求格式

![image-20210730161808822](https://gitee.com/cao_ziqiang/img/raw/master/20210730161808.png)

### 1.5 HTTP请求方法

![image-20210730161829350](https://gitee.com/cao_ziqiang/img/raw/master/20210730161829.png)

GET和POST请求方式的区别：

1. get请求参数是直接显示在地址栏的，而post在地址栏不显示；
2. get方式不安全，post安全；
3. get请求参数是又长度限制的，post没有限制

### 1.6 HTTP协议响应格式

![image-20210730162021227](https://gitee.com/cao_ziqiang/img/raw/master/20210730162021.png)

### 1.7 HTTP响应状态码

![image-20210730162045511](https://gitee.com/cao_ziqiang/img/raw/master/20210730162045.png)

常见的响应状态码：

- 200 OK //客户端请求成功

- 400 Bad Request //客户端请求有语法错误，不能被服务器所理解

- 401 Unauthorized //请求未经授权，这个状态代码必须和WWW-Authenticate 报头域一起使用

- 403 Forbidden //服务器收到请求，但是拒绝提供服务

- 404 Not Found //请求资源不存在，eg：输入了错误的 URL

- 500 Internal Server Error //服务器发生不可预期的错误

- 503 Server Unavailable //服务器当前不能处理客户端的请求，一段时间后可能恢复正常

## 二、Tomcat服务器

### 2.1 Tomcat简介

Tomcat服务器是一个免费的开放源代码的web应用服务器，属于轻量级应用服务器。

![image-20210730162205103](https://gitee.com/cao_ziqiang/img/raw/master/20210730162205.png)

JavaEE规范有13种，也称为"Java13绝技"，可以做简单了解，现在有了Spring框架简化开发，这些技术底层细节不用由开发者去过度关系，可以更多关心业务本身。

JDBC，JNDI，EJB，RMI，JSP，Servlets，XML，JMS，Java IDL，JTS，JTA，JavaMail，JAF。

Tomcat只实现了JSP和Servlets两个规范（<font color='red'>可参见libs目录，这也是为啥用JDBC要导包的原因</font>），所以也叫轻量级，但是实现了这两个也足够用了！！剩下的交给Spring！！！

再讲讲服务器：

虽然我们搞web开发的可能每天都离不开服务器，但是服务器是什么真的有清楚概念嘛？

<font color='red'>**我个人对于服务器的概念就能想到2点不同的分类：**</font>

- 软件概念的服务器和硬件概念的服务器

软件概念上，只要是一台硬件配置正常、装有操作系统、插着电能上网，并且安装特定软件的电脑，都可以称为服务器。比如你要学习数据库了，于是你装了MySQL服务端，那么此时你的电脑就是一个MySQL服务器。然后你又装了SVN服务端，那么此时你的电脑既是MySQL服务器，又是SVN服务器。Tomcat服务器同理。

硬件概念上，服务器本质上也是一台电脑，只不过配置高的同时长相丑了点，基本就是一个冰冷的大铁柜。我们的笔记本电脑既能看片又能玩游戏，而它们基本上专机专用。

- Web服务器？Web容器？

其实，Tomcat服务器 = Web服务器 + Servlet/JSP容器（Web容器）。

Web服务器的作用是接收客户端的请求，给客户端作出响应。但是很明显，服务器不止静态资源呀，还有我们写的Java程序，所以客户端发起请求后，如果是动态资源，Web服务器不可能直接把它响应回去（比如JSP），因为浏览器只认识静态资源。所以对于JavaWeb程序而言，还需要JSP/Servlet容器，**JSP/Servlet容器的基本功能是把动态资源转换成静态资源。**我们JavaWeb工程师需要使用Web服务器和JSP/Servlet容器，而通常这两者会集于一身，比如Tomcat。

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210730164535.jpeg)

Web服务器接收、响应客户端请求，Web容器装载Servlet/JSP，让它们去处理动态资源

为了更形象一点，我们假设我们在访问百度的网站，那么这次请求到最后在我们页面上显示的过程具体如下：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210730164829.jpeg)

这里我只是一个草图，如果想知道从url到页面的过程，欢迎去查看现有的一些八股文。

### 2.2 Tomcat目录结构

- \bin 存放启动和关闭 Tomcat 的可执行文件

- \conf 存放 Tomcat 的配置文件

- \lib 存放库文件

- \logs 存放日志文件

- \temp 存放临时文件

- \webapps 存放 web 应用

- \work 存放 JSP 转换后的 Servlet 文件

