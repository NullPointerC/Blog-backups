---
title: Django学习小记-4-视图
categories:
  - Django
tags:
  - Python
  - backend
  - Django
abbrlink: 36e4d102
date: 2021-08-19 00:01:21
---

>这里记录一下Django的视图和后台管理
>
>由于后台管理比较简单,都是一些定义,这里不再赘述

## 一、定义映射

### 1.1 基于函数的映射

首先将视图定义文件引入（import），然后利用path定义URL和视图的对应关系

### 1.2 基于类的视图

首先将视图类对象引入，之后用类似的方法配置URL和视图类的对应关系。

### 1.3 多App的场景

include将App的URL配置文件导入，之后就可以实现根据不同的App来分发请求，实现每个App自己管理自己的视图与URL映射，配置管理App的模式。

首先，在每个应用下创建urls.py文件，同时，修改项目的urls.py文件。

## 二、请求与响应

### 2.1 HttpRequest

每当请求到来的时候，Django就会创建一个携带有请求元数据的HttpRequest对象，传递给视图函数的第一个参数。

`method`：标识请求所使用的HTTP方法；

`scheme`：用来标识请求的协议类型；

`path`：返回当前请求页面的路径，但是不包括协议类型和域名；

`GET`：类字典对象（非dict,并且只读），包含GET请求中的所有参数；

`POST`：与GET类似，POST中保存的是POST请求中提交的表单数据；

`FILES`在上传文件时会用到，是一个类字典对象，包含所有上传文件数据；

`COOKIES`：字典（dict）对象，包含了一次请求的Cookie信息；

`META`是一个Python字典对象，包含了所有的HTTP头部信息

`user`标识当前登录用户的AUTH_USER_MODEL实例，它其实是Django用户系统中的User（auth.User）类型；

### 2.2 HttpResponse

`status_code`：用来标识一次请求的状态；

`content`：存储响应内容的二进制字符串；

`write方法`：这个方法将HttpResponse视为类文件对象，可以向其中添加响应数据；

`JsonResponse`：是最常用的子类对象，用于创建JSON编码的响应值；

`HttpResponseRedirect`：用于实现响应重定向，返回的状态码是302；

`HttpResponseNotFound`：继承自HttpResponse，只是将状态码修改为404；

## 三、基于类的视图

XxxView继承自View，它是所有基于类的视图的基类。其中定义了get和post方法，映射到GET和POST请求类型。

再回到项目目录下urls.py中配置path，需要用到View提供的as_vi ew方法完成URL的定义

## 四、动态路由

URL不是固定的，URL中包含了传递给视图的参数变量。

在方法中添加多个参数，并在url配置中`<int:year>`这也类似的规则

## 五、反向解析

### 5.1 reverse方法

配置URL模式的时候使用的path方法还可以接受一个name参数，用于指定当前URL模式的名字。

URL的反向解析就可以利用这个指定的name去完成。

reverse方法用于反向解析url，传入url的name即可，就可以得到完整的url，或向其中传递方法名也可以得到完整字符串。

### 5.2 命名空间

URL命名空间使得即使在不同的应用中定义了相同的URL名称，也能够反向解析到正确的URL。

URL命名空间分为两部分：应用命名空间（app_name）和实例命名空间（namespace）。

## 六、重定向

HttpResponseRedirect临时重定向，HttpResponsePermanentRedirect永久重定向；

HTTP对301的解释是被请求的资源已经永久移动到新的位置，将来任何对此资源的引用都应该使用本响应返回的若干个URI之一，它最常用到的场景是域名跳转；

302表示当前请求的资源临时从不同的URI响应，常用在同一个站点内的跳转，如未登录用户访问页面重定向到登录页；

## 七、通用视图

### 7.1 TemplateView

TemplateView是Django提供的一个最简单的通用视图，用于展示给定的模板

get_context_data。它返回一个字典对象，用于渲染模板上下文；

template_name。template_name用于指定模板路径，它是必须要提供的；

### 7.2 RedirectView

RedirectView用于实现通用重定向

permanent：标识是否使用永久重定向，默认是False。

url：重定向的地址。

pattern_name：重定向目标URL模式的名称（即path中的name参数）。

query_string：是否将查询字符串拼接到新地址中，默认为False，将丢弃原地址中的查询字符串。

get_redirect_url方法用于构造重定向的目标URL；