---
title: Django学习小记-8-中间件
categories: [Django]
tags:
  - Python
  - backend
  - Django
date: 2021-08-26 17:01:24
---

中间件是Django请求与响应处理的钩子框架，是一个轻量级的插件系统。

在Web开发中，钩子函数是很常见的，它可以实现触发和回调，并且做一些过滤和拦截。

Django的中间件用于在全局范围内改变输入（HttpRequest）和输出（HttpResponse）。

## 一、概念

### what?

中间件用于在视图函数执行之前和执行之后做一些预处理和后处理操作，功能类似装饰器。

中间件可以定义5个钩子函数，它们的名字是固定的。

### 钩子函数

1.process_request

请求预处理函数。在处理视图函数之前，Django会调用这个钩子函数，之后，根据函数返回值的不同会有不同的效果。

2.process_view

视图预处理函数。它会在确定了当前请求对应的视图函数之后被调用。

3.process_exception

异常后处理函数。当视图函数抛出了未捕获的异常时，这个钩子函数会被调用。

4.process_template_response

TemplateResponse或响应实例有render方法的后处理函数。

5.process_response

响应后处理函数。Django执行了视图函数并生成响应之后。

## 二、自定义

定义中间件最直接的方法是继承自`django.utils.deprecation.MiddlewareMixin`，并选择实现适合的钩子函数。

Django要求中间件必须要至少包含一个钩子函数，但即使是没有实现任何一个函数，尝试加载这个类也并不会报错，只是没有意义。

## 三、部署

### WSGI协议

Web Server Gateway Interface，Web服务器网关接口，它并不是服务器、框架或Python模块，而是一种规范或协议，它定义了Python Web应用程序与Web服务器通信的接口。

Python有各种各样的Web应用框架，在没有统一标准的情况下，可能需要针对每一个框架去实现各自的Web服务器。

WSGI的出现解决了这个问题，它可以让Web服务器知道如何去调用Python应用程序，把客户端的请求告诉应用程序；让Python应用程序知道客户端在请求什么，以及如何返回结果给Web服务器。

WSGI定义了两个角色：

Web服务器，被称作server或gateway；

应用程序，被称作application或framework。

server需要接受来自客户端的请求，然后根据协议的定义调用application；application处理请求，返回结果给server，并最终响应客户端。

### 生产环境

Django是一个Web应用框架，它并不会专注于实现服务器，所以，部署Django项目需要使用可以应用在生产环境的WSGI服务器。

可以使用Gunicorn或uWSGI作为server。

不论是uWSGI还是Gunicorn都不会直接对外提供服务，通常会使用Nginx作为服务器前端，接受用户请求。

Web服务器设计的目的是处理HTTP，而应用服务器既可以处理HTTP，也可以处理其他的协议，如RPC。

应用服务器一般都会集成Web服务器，但是通常对HTTP仅仅是支持，不会做特别的优化。因此，不会将应用服务器直接使用在生产环境中。

而上文提到的Gunicorn或uWSGI都是应用服务器，对HTTP的支持有限。

Web服务器适合提供静态内容，而应用服务器适合提供动态内容。因此，生产环境下会使用Web服务器作为应用服务器的反向代理。

所以，Gunicorn和uWSGI作为应用服务器实现对动态请求的处理，而Nginx作为Web服务器，用于处理静态文件并将动态请求反向代理给Gunicorn和uWSGI。



