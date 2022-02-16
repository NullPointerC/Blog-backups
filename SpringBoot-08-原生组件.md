---
title: SpringBoot-08-原生组件
categories:
  - SpringBoot
tags:
  - backend
  - SpringBoot
abbrlink: c2bec084
date: 2021-04-10 09:58:35
---

# Web原生组件注入

## 1、使用Servlet API

@ServletComponentScan(basePackages = **"com.atguigu.admin"**) :指定原生Servlet组件都放在那里

@WebServlet(urlPatterns = **"/my"**)：效果：直接响应，**没有经过Spring的拦截器？**

@WebFilter(urlPatterns={**"/css/\*"**,**"/images/\*"**})

@WebListener

## 2、使用RegistrationBean

# 嵌入式Servlet容器

## 切换容器

- 默认支持的webServer

	- `Tomcat`, `Jetty`, or `Undertow`
	- `ServletWebServerApplicationContext 容器启动寻找ServletWebServerFactory 并引导创建服务器`

web应用会创建一个web版的ioc容器 `ServletWebServerApplicationContext` 

- `ServletWebServerApplicationContext`  启动的时候寻找 **`ServletWebServerFactory`**`（Servlet 的web服务器工厂---> Servlet 的web服务器）`  
- SpringBoot底层默认有很多的WebServer工厂；`TomcatServletWebServerFactory`, `JettyServletWebServerFactory`, or `UndertowServletWebServerFactory`
- `底层直接会有一个自动配置类。ServletWebServerFactoryAutoConfiguration`
- `ServletWebServerFactoryAutoConfiguration导入了ServletWebServerFactoryConfiguration（配置类）`
- `ServletWebServerFactoryConfiguration 配置类 根据动态判断系统中到底导入了那个Web服务器的包。（默认是web-starter导入tomcat包），容器中就有 TomcatServletWebServerFactory`
- `TomcatServletWebServerFactory 创建出Tomcat服务器并启动；TomcatWebServer 的构造器拥有初始化方法initialize---this.tomcat.start();`
- `内嵌服务器，就是手动把启动服务器的代码调用（tomcat核心jar包存在）`

## 定制容器

- 实现  **WebServerFactoryCu**stomizer<ConfigurableServletWebServerFactory> 

	- 把配置文件的值和**`ServletWebServerFactory 进行绑定`**
- 修改配置文件 **server.xxx**
- 直接自定义 **ConfigurableServletWebServerFactory** 

**xxxxxCustomizer：定制化器，可以改变xxxx的默认规则**

```java
import org.springframework.boot.web.server.WebServerFactoryCustomizer;
import org.springframework.boot.web.servlet.server.ConfigurableServletWebServerFactory;
import org.springframework.stereotype.Component;
@Component
public class CustomizationBean implements WebServerFactoryCustomizer<ConfigurableServletWebServerFactory> {

    @Override
    public void customize(ConfigurableServletWebServerFactory server) {
        server.setPort(9000);
    }
}
```

