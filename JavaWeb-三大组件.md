---
title: JavaWeb-三大组件
date: 2021-07-30 16:54:59
categories: Java
tags: [web,Java,backend]
---

## 一、Servlet

### 1.1 Servlet简介

Servlet其实就是一种JavaEE编程技术，还是实现了特殊接口的Java类。

它由支持Servlet的Web服务器调用和启动运行，比如之前提到过的tomcat，这也是tomcat称为容器的原因。

一个Servlet负责对应的一个或一组URL访问请求，并返回相应的响应内容。

### 1.2 实现使用

1. 创建一个普通java文件

2. Java文件的类名实现HttpServlet
3. 重写service的方法
4. 在WEB-INF下的web.xml中添加请求与servlet类的映射关系

### 1.3 Servlet运行流程

![image-20210730170502174](https://gitee.com/cao_ziqiang/img/raw/master/20210730170502.png)

举个栗子：

我们访问这样的一个url：http://localhost:8080/firstweb/first

可以看到它的组成：

服务器地址：端口/虚拟项目名/servlet的别名

uri：虚拟项目名/servlet别名

过程：浏览器发送请求到服务器，服务器根据请求URL地址中的URI信息在webapps目录下找到对应的项目文件夹，然后再web.xml中检索对应的servlet，找到后调用并执行servlet。

### 1.4 生命周期

![image-20210730170702199](https://gitee.com/cao_ziqiang/img/raw/master/20210730170702.png)

其生命周期统计由tomcat管理，接收到请求后经过创建、init()、service()，输出响应信息后最终由tomcat回收。

**Service、doGet、doPost方法的区别：**

Service方法：不管是get还是post请求方式，如果service方法存在，则优先执行service方法

doGet方法：在没有service的情况下，如果是get请求，调用doGet方法

doPost方法：在没有service的情况下，如果是post请求，调用diPost方法

### 1.5 常见错误

- 404：访问资源不存在

–请求路径跟web.xml中填写的请求不一致

–请求路径的项目虚拟名称填写错误

- 405：

–请求的方式跟servlet中支持的方式不一致

- 500：服务器内部错误

–web.xml中servlet类的名称错误

–servlet对应的处理方法中存在代码逻辑错误

## 二、Request和Response

### 2.1 HttpServletRequest

在Tomcat中，封装了HttpServletRequest对象代表客户端的请求，当客户端通过HTTP协议访问服务器时，HTTP请求中的所有信息都封装在这个对象中，通过此种方式来获取HTTP请求中的数据。

### 2.2 request常用方法

```java
getRequestURL：获取客户端的完整URL
getRequesURI：获取请求行中的资源名部分
getQueryString：获取请求行的参数部分
getMethod：获取请求方式
getSchema：获取请求的协议
getRemoteAddr：获取客户端的ip地址
getRemoteHost：获取客户端的完整主机名
getRemotePort：获取客户端的网络端口号

获取请求头信息
getHeader(String name)
getHeaders(String name)
获取客户端请求参数（用户提交的数据）
getParameter(name)
getParameterValues(String name)
getParameterNames()
getParameterMap()
```

基本上都是见名知意的方法，需要用的时候查api即可。

### 2.3 HttpServletResponse

HttpServletResponse对象是tomcat封装的响应对象，这个对象中封装了向客户端发送数据，发送响应头，发送响应状态码的方法。



### 2.4 response常用方法

```java
设置响应头
setHeader(String key,String value)  添加响应信息，key重复会覆盖
addHeader(String key,String value)  添加响应信息，key重复不会覆盖
设置响应状态
sendError(int num,String msg )  添加响应状态
设置响应实体
getWriter().write(msg)   响应具体的数据给浏览器
```

### 三、Cookie

### 3.1 由来

因为HTTP是一个无状态的协议，当一个客户端向服务端发送请求，在服务器返回响应后，连接就关闭了，在服务器端不保留连接信息。

当客户端发送多次请求且需要相同的请求参数的时候，所以引入了Cookie机制在客户端保存HTTP状态信息。

### 3.2 Cookie简介

Cookie本身是在浏览器访问服务器的某个资源时，由web服务器在响应头传送给浏览器的数据。

假设浏览器如果保存了某个cookie，那么以后每次访问服务器的时候，都会在请求头传递给服务端。

一个cookie只能记录一种信息，是key-value形式。

一个web站点可以给浏览器发送多个cookie，一个浏览器也可以存储多个站点的cookie。

### 3.3 Cookie的基本原理

![image-20210730171844312](https://gitee.com/cao_ziqiang/img/raw/master/20210730171844.png)

### 3.4 cookie基本操作

```java
创建cookie对象
Cookie cookie = new Cookie(String key,String value);
设置cookie参数
cookie.setMaxAge(int seconds)
cookie.setPath(String url)
响应Cookie给客户端
response.addCookie(Cookie cookie)
获取cookie属性
Cookie[] cookies = request.getCookies()
```

## 四、Session

### 4.1 由来

和Cookie一样，Session的由来是因为虽然解决了客户端重复数据利用的问题，但是服务端也可能会有对一个用户不同请求的处理需要使用到相同的数据，所以就在服务端引入了Session技术。

### 4.2 Session简介

Session表示会话，在一段时间内，用户与服务器之间的一系列的交互操作。

session对象：用户发送不同请求的时候，在服务器端保存不同请求共享数据的存储对象。

特点：

Session是依赖cookie技术的服务器端的数据存储技术；

它由服务器进行创建，每个用户**独立拥有**一个session对象，默认存储时间是30分钟；

### 4.3 Session机制

用户使用浏览器第一次向服务器发送请求， 服务器在接受到请求后， 调用对应的 Servlet 进行处理。

在**这个处理过程中会给这个用户**创建一个 session 对象， 用来存储用户请求处理相关的公共数据， 并将此 session 对象的 JSESSIONID 以 sessoin 的形式存储在浏览器中(临时存储， 浏览器关闭即失效)。 

在以后用户在发起第二次请求及后续请求时， 请求信息中会附带 JSESSIONID， 服务器在接收到请求后，调用对应的 Servlet 进行请求处理， 同时根据 JSESSIONID 返回其对应的 session 对象；

![image-20210730172443622](https://gitee.com/cao_ziqiang/img/raw/master/20210730172443.png)

### 4.4 Session基本操作

```java
创建sessoin对象
HttpSession session = request.getSession()
向Session对象中添加值
sessoin.setAttribute(String name,Object object)
获取Session中的值
session.getAttribute(String name)
设置sessoin属性
session.setMaxInactiveInterval(5)//设置存活时间
session.invalidate(); //session强制失效
```

