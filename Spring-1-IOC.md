---
title: Spring-1-IOC
date: 2021-08-31 17:59:44
categories: FrameWork
tags: [FrameWork,backend,Spring,Java]

---

## IOC

IOC意为控制反转,是一种设计理念,在计算机中指把程序控制器交由第三方管理;

举个栗子:

在生活中诸如采购、租房,我们往往不会由自己去操作,通过第三方中介进行;

在Spring中,对象的创建和处理不由用户来创建,而是由代理人来创建和管理对象,用户通过代理人来获取对象;

IOC的目的在于降低对象之间的直接耦合,对象之间的关系可以灵活的变化,让对象关联变为弱耦合;

## DI

IOC是一种设计理念,是一个比较宏观的概念;

DI是IOC的一种具体技术实现,是微观的实现;

在Java中体现为利用反射技术实现对象注入;

## 优点

静态信息可以写在外部的配置文件中,而不用直接修改源代码,适合应用的更新迭代;

利用IOC容器让对象之间解耦合;

## 创建bean

### xml

创建`applicationContext.xml`管理bean;

IOC容器在`Spring`中以`ApplicationContext`对象的形式存在,`Spring`提供了`ClassPathXmlApplicationContext`对象来从**类路径**下读取配置文件并且创建IOC容器;

#### 构造器构造对象

直接书写bean标签而不添加其他属性时,spring会调用bean的无参构造器来实例化对象;

在bean标签中嵌套constructor-arg标签会使用带参构造器为对象实例化,并为字段设置值,可根据参数的名称title也可根据参数的位置index设值;

#### 静态工厂

仍然书写bean标签,但是`class`指向工厂的类路径,还有`factory-method`指向工厂方法;

#### 工厂实例

工厂实例方法创建对象是指IOC容器对工厂类进行实例化并且调用对应的实例方法创建对象的过程;

需要创建两个bean,首先创建工厂实例的bean,再创建实体的bean,`factory-bean`指向工厂实例,`factory-method`指向实例方法;



## 获取bean

context.getBean,有两种重载形式,一种是含有传入bean的字节码,一种是不含有字节码,获取bean后再进行强制类型转换;

