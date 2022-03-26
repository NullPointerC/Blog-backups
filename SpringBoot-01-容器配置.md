---
title: SpringBoot-01-容器配置
categories:
  - SpringBoot
tags:
  - backend
  - SpringBoot
abbrlink: 6e63cc9b
date: 2021-04-03 22:04:42
---

# 1、SpringBoot特点

## 1.1、依赖管理

父项目做依赖管理，几乎声明了所有开发中常用的依赖的版本号,自动版本仲裁机制；

开发导入starter场景启动器；

无需关注版本号，自动版本仲裁；

可以修改默认版本号；

## 1.2、自动配置


默认的包结构

- 主程序所在包及其所有子包里面的组件都会被默认扫描进来；
- 无需以前的包扫描配置想要改变扫描路径，@SpringBootApplication(scanBasePackages=**"com.atguigu"**)或者@ComponentScan 指定扫描路径

- @SpringBootApplication
	等同于
	@SpringBootConfiguration
	@EnableAutoConfiguration
	@ComponentScan("com.atguigu.boot")



# 2、容器功能

## 2.1、组件添加

### 1、@Configuration

**Full模式与Lite模式**

- 配置 类组件之间无依赖关系用Lite模式加速容器启动过程，减少判断
- 配置类组件之间有依赖关系，方法会被调用得到之前单实例组件，用Full模式

### 2、@Bean、@Component、@Controller、@Service、@Repository

注册bean，被IOC管理

### 3、@ComponentScan、@Import

```java
@Import({User.class, DBHelper.class})
// 给容器中自动创建出这两个类型的组件、默认组件的名字就是全类名
@Import({User.class, DBHelper.class})
@Configuration(proxyBeanMethods = false) //告诉SpringBoot这是一个配置类 == 配置文件
public class MyConfig {
}
```

### 4、@Conditional

条件装配：满足Conditional指定的条件，则进行组件注入

![image.png](http://static.codenote.xyz/img/1602835786727-28b6f936-62f5-4fd6-a6c5-ae690bd1e31d.png)

## 2.2、配置文件引入

### 1、@ImportResource

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <bean id="haha" class="com.atguigu.boot.bean.User">
        <property name="name" value="zhangsan"></property>
        <property name="age" value="18"></property>
    </bean>
    <bean id="hehe" class="com.atguigu.boot.bean.Pet">
        <property name="name" value="tomcat"></property>
    </bean>
</beans>
```

```java
@ImportResource("classpath:beans.xml")
public class MyConfig {}

======================测试=================
        boolean haha = run.containsBean("haha");
        boolean hehe = run.containsBean("hehe");
        System.out.println("haha："+haha);//true
        System.out.println("hehe："+hehe);//true
```

## 2.3、配置绑定 

如何使用Java读取到properties文件中的内容，并且把它封装到JavaBean中，以供随时使用；

```java
public class getProperties {
     public static void main(String[] args) throws FileNotFoundException, IOException {
         Properties pps = new Properties();
         pps.load(new FileInputStream("a.properties"));
         Enumeration enum1 = pps.propertyNames();//得到配置文件的名字
         while(enum1.hasMoreElements()) {
             String strKey = (String) enum1.nextElement();
             String strValue = pps.getProperty(strKey);
             System.out.println(strKey + "=" + strValue);
             //封装到JavaBean。
         }
     }
 }
```

### 1、@Component + @ConfigurationProperties

```java
/**
 * 只有在容器中的组件，才会拥有SpringBoot提供的强大功能
 */
@Component
@ConfigurationProperties(prefix = "mycar")
public class Car {
    private String brand;
    private Integer price;
    // 省略构造器、getter、setter
}
```

### 2、@EnableConfigurationProperties + @ConfigurationProperties

```java
@EnableConfigurationProperties(Car.class)
//1、开启Car配置绑定功能
//2、把这个Car这个组件自动注册到容器中
public class MyConfig {
}
```

