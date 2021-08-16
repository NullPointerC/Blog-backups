---
title: JavaWeb-Servlet其他组件
date: 2021-07-30 17:26:28
categories: Java
tags: [web,Java,backend]
---

## 一、ServletContext

### 1.1 由来

之前使用Cookie和Session分别解决了客户端和服务的相同数据共享的问题。但是对于服务端，往往不止有一个用户，要涉及不同用户的数据共享时，就需要使用ServletContext来作到数据共享。为整个的web应用提供一个上下文。

ServletContext是运行在JVM上的每一个web应用程序都有一个与之对应的Servlet上下文（Servlet运行环境）。

Servlet API提供ServletContext接口用来表示Servlet上下文，ServletContext对象可以被web应用程序中的所有servlet访问。

ServletContext对象是web服务器中的一个已知路径的根。

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210730214618.jpeg)

### 1.2 ServletContext原理

ServletContext 对象由**服务器进行创建**， **一个项目只有一个对象。** 

**不管在项目的任意位置进行获取得到的都是同一个对象**， 那么不同用户发起的请求获取到的也就是同一个对象了， 该对象由用户共同拥有。

### 1.3 Servletcontext API

![image-20210730213830188](https://gitee.com/cao_ziqiang/img/raw/master/20210730213830.png)

## 二、ServletConfig

### 2.1 由来

使用ServletContext对象可以获取web.xml中的全局配置文件。在web.xml中，有时每个Servlet也可以进行单独的配置，这时就需要使用到ServletConfig来获取配置信息。

### 2.2 作用

ServletConfig对象是Servlet的专属配置对象，每个Servlet都单独拥有一个ServletConfig对象 ，用来获取web.xml中的配置信息

### 2.3 使用

获取ServletConfig对象

获取web.xml中的servlet配置信息

## 三、JSP

### 3.1 为什么需要JSP

在上古时代，Servlet响应页面是通过输出流将HTML代码输出。

通常情况是需要前端写好html静态页面后，丢给Java程序员。Java程序员在Servlet中调用Service拿到数据后，**逐句复制**html静态页面上的html语句到Servlet的中，根据情况将后端的数据与html片段拼接在一起。

这样极大降低了开发效率，而同时期的PHP，ASP都支持在html中嵌入代码来生成动态数据。Java借鉴了二者的思想，在HTML语句中写入Java代码来生成动态数据，所以JSP应运而生。

所以PHP是世界上最好的语言！！！

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210730215600.gif)

### 3.2 JSP是什么

▪JSP是一种动态网页技术。当有人请求JSP时，服务器内部会经历一次动态资源（JSP）到静态资源（HTML）的转化，服务器会自动帮我们把JSP中的HTML片段和数据拼接成静态资源响应给浏览器。也就是说JSP是运行在服务器端，但最终发给客户端的都已经是转换好的HTML静态页面（在响应体里）。

即：**JSP = HTML + Java片段**（各种标签本质上还是Java片段）。

JSP本质是一个Java类（Servlet），是在服务器混的，只不过它输出结果是HTML。

每个JSP 页面在第一次被访问时，JSP引擎将它翻译成一个Servlet源程序，接着再把这个Servlet源程序编译成Servlet的class类文件，通过JSP引擎将JSP转译成Servlet，然后再由WEB容器像调用普通Servlet程序一样的方式来装载和解释执行这个由JSP页面翻译成的Servlet程序。

### 3.3 JSP执行过程

![image-20210730220305037](https://gitee.com/cao_ziqiang/img/raw/master/20210730220305.png)

1. 浏览器输入：localhost:8080/jsp/1.jsp；
2. tomcat收到*.jsp请求，则会到org.apache.jasper.servlet.JspServlet处理；
3. JspServlet调用相应的java类，jsp引擎根据_1_.jsp生成的java类（位于tomcat子目录work下面：_1_jsp.java,_1_jsp.class）；
4. 执行_1_.jsp.class,将HTML数据输出给tomcat，tomcat再将这些数据输出给客户端

### 3.4 JSP和Servlet二者渲染对比

Servlet:

优点：逻辑处理方便

缺点：页面表现麻烦

JSP：

优点：页面表现方便

缺点：逻辑处理麻烦

### 3.5 JSP和AJAX

其实请求、响应这么一来一回，无非要的就两样东西：数据+HTML骨架。如果把服务器端比作淘宝卖家，客户端（浏览器）比作买家，而数据和HTML则是一件商品的两个重要组成部件。那么我们很自然地能够想到，其实运输方式至少可以有两种：

1.卖家组装好商品后再发货（JSP）

2.卖家把部件拆开，运到之后买家自己组装（AJAX+HTML）

JSP是服务器端的，它的局限性在于数据必须在返回给客户端之前就“装载”完毕。不然HTML都已经发出去了，你就没办法跑到浏览器里把数据给它安上。

而有了AJAX，就可以实现零件发送、目的地组装了。

### 3.6 JSP语法

编译器指令

```jsp
page
<%@ page contentType="text/html; charset=GBK"%>
<%@ page import="java.util.*, java.lang.*" %> 
<%@ page errorPage="error.jsp" %>

include
<%@ include file=“相对位置” %>

taglib
<%@ taglib uri="http://java.sun.com/jstl/core" prefix="c"%>
```

脚本语法

```jsp
“HTML注释”:<!— comments -->
Servlet中会生成，会发给浏览器
    
“隐藏注释”:<%-- comments --%>
Servlet中不生成，不发给浏览器
    
“声明”
<%! 声明; [声明; ] ... %>
    
“表达式”
<%=…%>
    
“脚本段”
<%...%>
```

include

```java
静态导入：
<%@ include file="logo.jsp" %>
是在servlet引擎转译时，就把此文件内容包含了进去（两个文件的源代码整合到一起，全部放到_jspService方法中），所以只生成了一个servlet，所以两个页面不能有同名的变量。
运行效率高一点点。耦合性较高，不够灵活。
    
动态导入
<jsp:include page=“logo.jsp”></jsp:include>
是在servlet引擎转译后，请求的此页面，所以共生成了两个servlet，所以可以有同名变量。
生成两个servlet，相当于两个类之间的调用，耦合性较低，非常灵活。
```

动作语法-forword请求转发

```java
<jsp:forward>用于请求转发！标签以后的代码，将不会被执行！
```

![image-20210731102614773](https://gitee.com/cao_ziqiang/img/raw/master/20210731102614.png)

JSP九大内置对象

```java
pageContext
一个页面，当前页面
request
一次请求所有被转发过的servlet
session
一次会话所有的servlet
application
一个项目所有的servlet
response
相应信息，比较底层，没有做封装
out
内置了一个缓冲区，响应信息推荐使用out
config
配置信息，很少使用
page
当前页面对象，基本不用
exception
异常对象，根本不用
```

## 四、EL和JSTL

### 4.1 EL表达式

```java
概念：
Expression Language，一种写法非常简单的表达式，语法简单易懂，便于使用。

作用：
让jsp书写起来更加的方便。简化在jsp中获取作用域或者请求数据的写法

语法结构
${expression}
提供.和[]两种运算符来获取数据
```

EL表达式用法

```java
使用EL表达式获取请求数据
	获取用户请求数据
	获取请求头数据
	获取cookie数据
使用EL表达式获取作用域数据
	获取作用域数据
	作用域查找顺序
	获取指定作用域中的数据
使用EL表达式进行运算
	算术运算
	关系运算
	逻辑运算
```

### 4.2 JSTL标签库

JSTL 是 apache 对 EL 表达式的扩展（也就是说 JSTL 依赖 EL），JSTL 是标签语言！ JSTL 标签使用以来非常方便， 它与 JSP 动作标签一样， 只不过它不是 JSP 内置的标签， 需要我们自己导包， 以及指定标签库而已！

JSTL标签库的作用：

用来提升在 JSP 页面的逻辑代码的编码效率， 使用标签来替换逻辑代码的直接书写， 高效， 美观， 整洁， 易读。

使用方式：

```jsp
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

JSTL标签库的用法:

```java
核心标签库
格式化标签库
函数标签库
XML标签库
SQL标签库
```

## 五、过滤器

### 5.1 过滤器的概念

过滤器是能够对web请求和web响应的头属性和内容体进行操作的一种特殊web组件；

过滤器的特殊之处在于本身并不直接生成web响应，而是拦截web请求和响应，以便查看、提取或以某种方式操作客户机和服务器之间交换的数据

### 5.2 过滤器的功能

- 分析web请求，对输入数据进行预处理
- 阻止web请求和响应的进行
- 根据功能改动请求的头信息和数据体
- 与其他web资源协作

### 5.3 过滤器工作原理

![image-20210731103151228](https://gitee.com/cao_ziqiang/img/raw/master/20210731103356.png)

### 5.4 过滤器的使用方法

- 过滤器的API包括javax.servlet包中的Filter，FilterChain和FilterConfig三个接口
- 所有的过滤器都必须实现javax.servlet.Filter接口，该接口定义了init()、doFilter()和destory()三个方法
- 这三个方法分别对应了过滤器生命周期中的初始化、响应和销毁这三个阶段

### 5.5 过滤器的生命周期

实例化	->	初始化	->	过滤	->	销毁

### 5.6 过滤器链

FilterChain接口用于调用过滤器链中的一系列过滤器

![image-20210731103548194](https://gitee.com/cao_ziqiang/img/raw/master/20210731103548.png)

FilterChain接口

- 对于一个Servlet，用户可以定义多个Filter。这些Filter由容器组织成一个过滤器链。在每个Filter对象中，可以使用容器传入doFilter方法的FilterChain参数引用该过滤器链。
- FilterChain接口定义了一个doFilter方法，用于将请求/响应继续沿链向后传送
- public void doFilter(ServletReqluest request,ServletResponse response)

## 六、监听器

### 6.1 监听器概念

- Servlet监听器用于监听一些重要事件的发生，监听器对象可以在事情发生前、发生后可以做一些必要的处理
- 通过实现Servlet API提供的Listense接口，可以在监听正在执行的某一个程序，并且根据程序的需求做出适当的响应

### 6.2 Servlet事件监听器

- Servlet2.3提供了对ServletContext和HttpSession对象的状态变化的监听器，Servlet2.4则增加了对ServletRequest对象状态变化的监听器

- ServletContext对象----监听ServletContext对象，可以使web应用得知web组件的加载和卸载等运行情况

- HttpSession对象----监听HttpSession对象，可以使web应用了解会话期间的状态并做出反应

- ServletRequest对象----监听ServletRequest对象，可以使web应用控制web请求的生命周期

事件监听器的类型

| **监听对象**   | **监听器接口**                                           | **监听事件**                                      |
| -------------- | -------------------------------------------------------- | ------------------------------------------------- |
| ServletContext | ServletContextListener  ServletContextAttributeListener  | ServletContextEvent  ServletContextAttributeEvent |
| HttpSession    | HttpSessionListener  HttpSessionActivationListener       | HttpSessionEvent                                  |
| HttpSession    | HttpSessionAttributeListener  HttpSessionBindingListener | HttpSessionBindingEvent                           |
| ServletRequest | ServletRequestListener  ServletRequestAttributeListener  | ServletRequestEvent  ServletRequestAttributeEvent |