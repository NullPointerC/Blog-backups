---
title: SpringBoot-05-请求参数处理
categories:
  - SpringBoot
tags:
  - backend
  - SpringBoot
abbrlink: 99daf8ba
date: 2021-04-08 09:58:35
---

# 1、请求参数处理

## 1.1 、请求映射

![image.png](http://static.codenote.xyz/img/20210821100100.png)

DispatcherServlet	->	doDispatch（）

```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
	HttpServletRequest processedRequest = request;
	HandlerExecutionChain mappedHandler = null;
	boolean multipartRequestParsed = false;	
	WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);
	try {
		ModelAndView mv = null;
		Exception dispatchException = null;
		try {
			processedRequest = checkMultipart(request);
			multipartRequestParsed = (processedRequest != request);
			// 找到当前请求使用哪个Handler（Controller的方法）处理
			mappedHandler = getHandler(processedRequest);
            //HandlerMapping：处理器映射。/xxx->>xxxx
```
![image.png](http://static.codenote.xyz/img/20210821100310.png)

**RequestMappingHandlerMapping**：保存了所有@RequestMapping 和handler的映射规则。

## 1.2、普通参数与基本注解

### 注解：

@PathVariable、@RequestHeader、@ModelAttribute、@RequestParam、@MatrixVariable、@CookieValue、@RequestBody

### Servlet API：

WebRequest、ServletRequest、MultipartRequest、 HttpSession、javax.servlet.http.PushBuilder、Principal、InputStream、Reader、HttpMethod、Locale、TimeZone、ZoneId

### 复杂参数：

**Map**、**Model（map、model里面的数据会被放在request的请求域  request.setAttribute）、**

Errors/BindingResult、**RedirectAttributes（ 重定向携带数据）**、**ServletResponse（response）**、

SessionStatus、UriComponentsBuilder、ServletUriComponentsBuilder

**Map、Model类型的参数**，会返回 mavContainer.getModel（）；---> BindingAwareModelMap 是Model 也是Map

**mavContainer**.getModel(); 获取到值的

### 自定义对象参数：

可以自动类型转换与格式化，可以级联封装。

## 1.3 、参数处理原理

- HandlerMapping中找到能处理请求的Handler（Controller.method()）
- 为当前Handler 找一个适配器 HandlerAdapter； **RequestMappingHandlerAdapter**
- 适配器执行目标方法并确定方法参数的每一个值
- 执行目标方法
- 参数解析器HandlerMethodArgumentResolver确定将要执行的目标方法的每一个参数的值是什么;
- 目标方法执行完成，处理派发结果



