---
title: Spring-3-注解开发
date: 2021-09-03 19:40:38
categories: FrameWork
tags: [FrameWork,backend,Spring,Java]

---

## 注解开发

可以摆脱繁琐的XML形式的bean与依赖注入配置;

基于"声明式"的原则,更适合轻量级的现代化应用;

代码可读性更高,也可以加快开发人员的开发速度;

### 注解分类

#### 组件类型注解-声明当前类的功能与职责;

@Component,@Controller,@Service,@Repository

这类注解都是语义类注解,说明当前类的职责,功能都是一样;

开启组件扫描

```xml
<context:component-scan base-package="com.xxx">
    <!--即排除com.xxx.xxx包下的所有类-->
	<context:exclued-filter type="regex" expression="com.xxx.xxx">
</comtext:component-scan>
```

#### 自动装配注解-根据属性特征自动注入对象;

按照类型装配@Autowired、@Inject

@Autowired注解如果放置在属性名上，spring的ioc容器会自动通过反射技术将属性private修饰符改为public，直接进行赋值；

如果放置在set方法上，则自动按类型、名称对set方法参数进行注入；

按类型注入碰到问题如此类型的类不唯一时，可以只对要注入的类设置@Repository注解或加上@Primary注解；

@Inject注解基于JSR-330标准，其他同@Autowired，但是不支持required属性；

@Autowired有个属性为required，可以配置为false，如果配置为false之后，当没有找到相应bean的时候，系统不会抛错；

按照名称装配@Name、@Resource

推荐使用@Resource注解，首先如果设置了name属性，则按照name属性在ioc容器中注入，如果未设置name属性，将会先以属性名作为bean name在ioc容器中匹配bean，如果有匹配则注入，如果属性名未匹配，则按照类型匹配，这时功能同@Autowired，需要加入@Primary解决类型冲突；

#### 元数据注解-更细化的辅助IoC容器管理对象的注解;

| 注解           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| @Primary       | 按类型装配时，出现多个相同类型的对象，拥有此注解对象优先被注入 |
| @PostConstruct | 描述方法，相当于xml中的init-method配置的注解版本             |
| @PreDestroy    | 描述方法，相当于xml中的destroy-method配置的注解版本          |
| @Scope         | 设置对象的scope属性                                          |
| @Value         | 为属性注入静态数据                                           |

## Java Config

可以完全摆脱xml的束缚，使用独立的Java类管理对象和依赖；

注解配置相对分散，而使用Java Config可对配置集中管理；

可以在编译时进行依赖检查，不容易出错；

### 核心注解

| 注解            | 说明                                                        |
| --------------- | ----------------------------------------------------------- |
| @Configuration  | 描述类，说明当前类是Java Config配置类，完全用来替代xml文件  |
| @Bean           | 描述方法，方法的返回值将会被IOC容器管理，beanId默认为方法名 |
| @ImportResource | 描述类，加载配置文件，可使用@Value注解获取                  |
| @ComponentScan  | 描述类，同xml的< context:component-scan >标签               |

### 依赖注入

在方法参数中，加入所依赖的bean，ioc会自动把所依赖的bean注入

需要几个依赖，就可以在方法签名中声明这些依赖，ioc容器会自动将bean注入。



