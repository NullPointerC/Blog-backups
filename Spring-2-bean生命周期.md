---
title: Spring-2-bean生命周期
date: 2021-09-03 18:53:05
categories: FrameWork
tags: [FrameWork,backend,Spring,Java]

---

## bean scope

bean scope属性用于决定对象何时被创建与作用范围;

bean scope配置将会影响容器内对象的数量;

bean scope默认值singleton(单例),值全局共享同一个对象实例;

### 分类

| 属性        | 范围说明                                                 |
| ----------- | -------------------------------------------------------- |
| singleton   | 单例(默认值),每一个容器有且只有唯一的实例,实例被全局共享 |
| prototype   | 多例,每次使用时都是创建一个实例                          |
| request     | web环境下,每一次独立请求存在唯一实例                     |
| session     | web环境下,每一个session存在有唯一实例                    |
| application | web环境下,ServletContext存在唯一实例                     |
| websocket   | 每一次WebSocket连接中存在唯一实例                        |

singleton占用资源少,但是存在线程安全问题,在IoC容器启动时就会实例化对象;

prototype占用资源多,不存在线程安全问题,在getBean()或者对象注入时才会实例化对象;

### bean生命周期

IoC容器准备初始化解析XML;

对象实例化,执行构造方法;

为对象注入属性;

调用init-method初始化方法;

IoC容器初始化完毕;

执行业务代码;

IoC容器准备销毁;

调用destroy-method释放资源;

IoC容器销毁完毕;

### IoC本质

本质就是一个map,对应beanId	->	obj;





