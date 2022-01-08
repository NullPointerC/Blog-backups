---
title: SpringBoot-配置文件
date: 2021-12-25 19:43:26
categories: [SpringBoot]
tags:
  - backend
  - SpringBoot
---

## properties

$SprintBoot$对于传统$Spring$的一个很大的优点就在于比较方便的配置文件，在这里总结一下。

在$boot$中可以有$3$种配置文件。

最常见的就是$application.properties$文件，也是书写常见的键值对形式，也是我一直以来都比较喜欢的一种配置文件形式，当时觉得$yml$的格式太丑了，而且可能缩进看上去不太舒服。

在$application.properties$中书写$key-value$形式的配置形式，配置也一般以前缀形式出现，如果不太记得，也可以去$spring$的[官网](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#application-properties)查看，比如服务器$tomcat$的配置一般以$server$开头，日志配置以$logging$开头等。

同时，$boot$也配置了大量默认值，很多值也在引入的$starter$中配置好了。

另外，$boot$提供了另外的两种格式，$yml$和$yaml$格式。

如果$boot$中三种配置文件都配置了，优先级而言$properties \gt yml \gt yaml$，不同配置文件中相同配置按照优先级相互覆盖，不同文件的不同配置则全部保留。

## yaml

$yaml$是一种新的数据序列化形式，有许多优点，容易阅读，并且容易和脚本语言交互，以数据为中心，重数据轻格式。有两种拓展名，$yml$和$yaml$都是正确的拓展名，但是一般来说前者更加主流。

规则：

- 大小写敏感；
- 属性的层级关系使用多行来描述，每行结尾使用冒号结束；
- 使用缩进代表层级关系，同层次左侧对齐，只允许使用空格，不能使用$tab$；
- 属性值前面要有一个空格，名和值之间使用冒号+空格进行分割；
- #代表注释；

核心规则：数据前面要有空格

字面值表示方式：

```yaml
boolean: TRUE                        #TRUE,true,True,False,false,FALSE均可
float: 3.14                          #6.8523015e+5支持科学计数法
int: 123                             #0b1010_0110_1010_1110 支持二进制、八进制、十六进制
null: ~                              #使用~表示null
string: HelloWorld                   #字符串可以直接书写
string2: "HelloWorld"                #可以用双引号包裹特殊字符
date: 2021-12-25                     #日期必须用yyyy-MM-dd格式
datetime: 2018-02-17T15:02:31+08:00  #日期与时间之间使用T连接，最后用+代表时区
```

数组表示形式：

在属性名书写位置下方使用减号作为数据的开始符号，每行书写一个数据，减号与数据之间空格分隔

```yaml
data:
  - 1
  - 2
  - 3
 subject:
  - Java
  - 前端
  - 运维
  - 测试
enterprise:
  name: zhangsan
  age: 20
  subject:
  	- Java
  	- 计组
  	- 计网
 
 likes: [吃饭,睡觉,打豆豆] #数组简略形式
```

对象数据格式：

```yaml
users
  - name: Tom
  	age: 4
  - name: Jack
  	age: 5
  - name: zhangsan
  	age: 6
 users1:
  -
  	name: Tom
  	age: 33
  -
  	name: Jack
  	age: 44
 users2: [ {name:zhangsan, age:4}, {name:lisi, age:5}] #对象数组简略形式
```

## boot中读配置

### 单一数据

使用$@Value$注解和$SpEL$表达式读取配置的值如$@Value(\$\{name\})$即可读取到默认配置文件中名为$name$的配置信息。

表示层级关系时，使用$.$即可，如$@Value(\$\{user.name\})$即可读取到$user.name$这个配置的值。

读取数组中的某一项时，$@Value(\$\{likes[0]\})$即可读取到$likes$数组中的第$1$项。

### 变量引用

在$yaml$中可以设置变量的形式来减少同一前缀的配置项书写。

如：

```yaml
baseDir: C:\Windows
tempDir: ${baseDir}\temp
```

这样就可以引用定义的$baseDir$，更加方便的修改了。

```yaml
mrbird
  blog
    name: mrbird's blog
	title: Spring Boot
	wholeTitle: ${mrbird.blog.name}--${mrbird.blog.title}
```



### 全部数据

读取配置文件中的全部配置信息，$boot$提供了$Environment$对象进行配置信息的封装，这其中封装了配置文件中的全部信息。

可以使用$getProperty$方法获取配置的值。

### 对象数据

对于一个完整的配置项来说，配置的$SpEL$表达式可能会很长，不方便记忆，所以可以使用配置的对象形式来加载配置信息。通常使用$@ConfigurationProperties(prefix="xxx.yyy")$来加载。

可以预先定义好对象用于封装配置文件中的数据，由$spring$来加载数据到对象中。

创建的类要有$getter$和$setter$方法，且属性名要对应上，且类要加入$spring$容器来管理，在配置类上加入$@Component$注解使其注入到容器中，或是在需要用到这个封装类的地方使用$@EnableConfigurationProperties$来注入配置信息，该注解用于开启对$@ConfigurationProperties$的支持。

`@EnableConfigurationProperties` 文档中解释：
 当`@EnableConfigurationProperties`注解应用到你的`@Configuration`时， 任何被`@ConfigurationProperties`注解的beans将自动被Environment属性配置。 这种风格的配置特别适合与SpringApplication的外部YAML配置进行配合使用。

在类上方配置$@ConfigurationProperties$指定加载的数据，注解中值为配置的前缀$prefix$。

如下所示：

```java
@Data
@ConfiguraionProperties(prefix = "datasource")
@Component
public class MyDataSource {
	private String driver;
	private String url;
	private String username;
	private String password;
}
```

### 更换配置来源

在$boot$中通常默认配置信息是从$application.properties$中读取，但是对于有些信息，也可以配置在其他文件中，使用$@PropertySource(path="classpath:xxx.properties")$的方式执行配置来源后，再配置$@ConfigurationProperties$来指定前缀读取配置信息。

### 常用注解

$@PorpertySource$

用于向容器中导入自定义配置信息，一般用于导入$.properties$的属性配置文件，无法解析$.yml$文件。

$@ImportResource$

也用于向容器中导入$Bean$以及配置信息，一般用于导入$.xml$的属性配置文件。在$boot$中用得比较少，已经被配置类所取代，如果要在$boot$中使用，要放在配置启动类上。

$@Configuration$

用来标注该类是作为$bean$定义的源，相当于配置$<beans>$标签。此外该注解标注的类允许通过调用同一类的其他$@Bean$标注的方法表示$bean$之间的依赖关系。

$@Bean$

标注在方法上，用于指示方法的返回值的实例化、配置和初始化都由IOC容器来管理。

比较通俗的解释是$@Configuration$标注的类等同于一个$xml$文件，而$@Bean$标注的方法等同于一个$<bean>$标签。

$@ConfigurationProperties$

将配置文件中以指定前缀的配置进行导入并绑定，作用与$@Value$类似，但是更加方便。

$@EnableConfigurationProperties$

使得配置了$@ConfiguraionProperties$的配置类生效，即引入该配置类，这样前者的配置类可以不用交由IOC容器来管理。

