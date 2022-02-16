---
title: SpringBoot-06-请求响应解析
categories:
  - SpringBoot
tags:
  - backend
  - SpringBoot
abbrlink: ecf3e520
date: 2021-04-09 09:58:35
---

# 1、数据响应

## 1.4、数据响应与内容协商

### 响应JSON

jackson.jar+@ResponseBody

返回值解析器原理

1、返回值处理器判断是否支持这种类型返回值 supportsReturnType

2、返回值处理器调用 handleReturnValue 进行处理

3、RequestResponseBodyMethodProcessor 可以处理返回值标了@ResponseBody 注解的。

​		利用 MessageConverters 进行处理 将数据写为json

​		1、内容协商（浏览器默认会以请求头的方式告诉服务器他能接受什么样的内容类型）

​		2、服务器最终根据自己自身的能力，决定服务器能生产出什么样内容类型的数据，

​		3、SpringMVC会挨个遍历所有容器底层的 HttpMessageConverter ，看谁能处理？

​			1、得到MappingJackson2HttpMessageConverter可以将对象写为json

​			2、利用MappingJackson2HttpMessageConverter将对象转为json再写出去。

### 

## 1.5、视图解析与模板引擎

### 视图解析原理流程

**1、目标方法处理的过程中，所有数据都会被放在 ModelAndViewContainer 里面。包括数据和视图地址**

**2、方法的参数是一个自定义类型对象（从请求参数中确定的），把他重新放在** **ModelAndViewContainer** 

**3、任何目标方法执行完成以后都会返回 ModelAndView（数据和视图地址）。**

**4、processDispatchResult  处理派发结果（页面改如何响应）**

### 模板引擎-Thymeleaf

**现代化、服务端Java模板引擎**

| 表达式名字 | 语法   | 用途                               |
| ---------- | ------ | ---------------------------------- |
| 变量取值   | ${...} | 获取请求域、session域、对象等值    |
| 选择变量   | *{...} | 获取上下文对象值                   |
| 消息       | #{...} | 获取国际化等值                     |
| 链接       | @{...} | 生成链接                           |
| 片段表达式 | ~{...} | jsp:include 作用，引入公共页面片段 |

## 1.6、拦截器

### HandlerInterceptor 接口

```java
public class XxxInterceptor implements HandlerInterceptor {
	 /**
     * 目标方法执行之前
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        // do something
    }
    /**
     * 目标方法执行完成以后
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        // do something
    }
    /**
     * 页面渲染以后
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        // do something
    }
}
```

### 配置拦截器

```java
/**
 * 1、编写一个拦截器实现HandlerInterceptor接口
 * 2、拦截器注册到容器中（实现WebMvcConfigurer的addInterceptors）
 * 3、指定拦截规则【如果是拦截所有，静态资源也会被拦截】
 */
@Configuration
public class AdminWebConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginInterceptor())
                .addPathPatterns("/**")  //所有请求都被拦截包括静态资源
                .excludePathPatterns("/","/login","/css/**","/fonts/**","/images/**","/js/**"); //放行的请求
    }
}
```

### 拦截器原理

1、根据当前请求，找到**HandlerExecutionChain【**可以处理请求的handler以及handler的所有 拦截器】

2、先来**顺序执行** 所有拦截器的 preHandle方法

​	1、如果当前拦截器prehandler返回为true。则执行下一个拦截器的preHandle

​	2、如果当前拦截器返回为false。直接   倒序执行所有已经执行了的拦截器的  afterCompletion；

**3、如果任何一个拦截器返回false。直接跳出不执行目标方法**

**4、所有拦截器都返回True。执行目标方法**

**5、倒序执行所有拦截器的postHandle方法。**

**6、前面的步骤有任何异常都会直接倒序触发** afterCompletion

7、页面成功渲染完成以后，也会倒序触发 afterCompletion

