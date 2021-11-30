---
title: SpringBoot-日志
categories: [SpringBoot]
tags:
  - backend
  - SpringBoot
date: 2021-05-18 18:30:02
---

## 1. 日志框架

### 日志的起源：

① 在系统开发过程中将关键的数据输出到控制台；

② 系统中过多的控制台输出语句不便于维护，将控制台输出语句整合成日志jar包

`xxxlogging.jar`；

③ 对日志jar包升级产生新的日志jar包，`xxxlogging-update.jar` 面临对系统中已经使用的日志jar包替换问题；

④ 参考JDBC和数据库驱动之间的关系（JDBC只是调用接口，各大数据库厂商提供实现类）创建日志门面（日志的抽象层）`logging-abstract.jar`；

⑤ 项目中使用日志只需要导入具体的日志实现即可，日志框架都实现了日志抽象层；

### 市面上常见的日志框架：

JUL、JCL、Jboss-logging、logback、log4j、log4j2、slf4j......

### 框架简介:

|             日志框架名称              | 所属类型 |                      简介                       |
| :-----------------------------------: | :------: | :---------------------------------------------: |
|     JCL(Jakarta Commons Logging)      | 日志门面 |           Apache提供的一个通用日志API           |
| SLF4j(Simple Logging Facade for Java) | 日志门面 |         SLF4J提供了统一的记录日志的接口         |
|             jboss-logging             | 日志门面 |    jboss-logging是一款类似于slf4j的日志框架     |
|          Log4j(Log for java)          | 日志实现 |       Apache基金会一款优秀的开源日志框架        |
|        JUL(java.util.logging)         | 日志实现 |               JDK中提供的日志输出               |
|                Log4j2                 | 日志实现 | Apache基金会推出的Log4J的升级版，支持插件式结构 |
|                Logback                | 日志实现 | Logback是由log4j创始人设计的又一个开源日志框架  |

- 在使用日志框架时，选择一个日志门面作为调用接口，选择一个日志框架实现作为具体实现；
- Spring Boot的底层Spring FrameWork框架默认使用的JCL框架；
- 而Spring Boot 中Starter整合的日志门面是SLF4j，日志实现选用的是logback；

## 2. SLF4J框架

SLF4J（Simple Logging Facade for Java）即简单日志门面，不是具体的日志解决方案，它只服务于各种各样的日志系统。按照官方的说法，SLF4J是一个用于日志系统的简单Facade，允许最终用户在部署其应用时使用其所希望的日志系统。

SLF4J 是一个日志抽象层，允许你使用任何一个日志系统，并且可以随时切换其他日志框架还不需要修改已经写好的程序。SLF4J只负责制定日志标准，并不是日志系统的具体实现。SLF4J只负责两件事情：

① 提供日志接口；

② 提供获取具体日志对象的方法；

### SLF4j框架的使用:

我们在使用日志调用日志中的方法时，应当调用日志抽象层的方法，而不是日志的具体实现类。

SLF4J框架使用步骤：

① 导入SLF4J的jar包和logback的实现jar包；

② 使用SLF4J提供的方法使用日志；

官方代码示例：

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```

- 我们仅导入 slf4j-api.jar 包是不能实现日志操作的。SLF4J只提供日志操作接口，不提供具体的操作方法，如果想使用SLF4J作为日志的操作的门面还需要导入具体的日志实现框架；

- 例如：使用SLF4J框架和Logback框架共同完成日志操作，则需要导入 slf4j-api.jar logback-classic.jar logback-core.jar 包，即：使用SLF4J框架作为日志操作的门面，而Logback框架作为日志操作的具体实现类；

- 如果使用了不是直接实现SLF4J框架的日志实现类则需要引入中间适配包进行整合调用；

- 例如：使用SLF4J框架和log4j框架完成日志操作，除了依赖 slf4-api.jar log4j.jar 包外还需要依赖适配包 slf4j-log412.jar；

SLF4J框架与其他日志框架整合时需要依赖的jar包关系如下图所示：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210518183547.png)

每种日志实现框架都有自己的配置文件，在使用SLF4J后，配置文件还是书写成日志实现框架本身的配置文件。

SLF4J的官方文档：<a href="http://www.slf4j.org/manual.html">SLF4J使用手册</a>

### SLF4j框架统一日志输出:

当我们在进行框架整合时会遇到不同的框架使用的日志输出框架不统一的问题，需要我们进行日志框架的统一。

例如：我们的系统开发计划使用slf4j和logback日志框架做日志输出，我们的项目使用SSH框架。但是Spring Framework框架默认的日志输出是commons-logging框架，Hibernate框架默认的日志输出是jboss-logging框架，这时就需要我们对SLF4j框架的日志统一输出，屏蔽系统中其他日志输出框架。

如何让系统中所有的日志都统一到slf4:

① 将系统中其他日志框架先排除出去；

② 用中间包来替换原有的日志框架；

③ 我们导入slf4j其他的实现；

例如：在项目中使用slf4j作为日志的门面框架，logback作为日志的实现框架，需要导入slf4j和logback相关的jar包。但项目中整合的Spring Frame框架使用的日志框架为JCL，此时需要将JCL日志框架在项目中排除掉，引入中间

包 `jcl-over-slfj4.jar` 替换原来JCL的jar包。这样项目中的日志输出也就统一到slf4j为日志的门面框架，logback为日志的实现框架了。

使用slf4j统一日志框架图示：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210518183712.png)

相关链接：<a href="http://www.slf4j.org/legacy.html">slf4j解决遗留问题</a>

## 3. Spring Boot与日志

### Spring Boot中日志的配置

`pom.xml` 中导入 了`spring-boot-starter-web` 依赖；

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

`spring-boot-starter-web` 依赖中包含 `spring-boot-stater` 依赖；

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <version>2.3.4.RELEASE</version>
    <scope>compile</scope>
</dependency>
```

`spring-boot-stater` 依赖中包含 `spring-boot-starter-logging` 依赖用于配置日志相关；

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-logging</artifactId>
    <version>2.3.4.RELEASE</version>
    <scope>compile</scope>
</dependency>
```

`spring-boot-starter-logging` 依赖的构成

![image-20201020094345703](https://gitee.com/cao_ziqiang/img/raw/master/20210518183922.png)

Spring Boot底层使用 `slf4j + logback` 作为日志输出，并对整合的其他框架中默认的日志框架进行替换，实现日志输出框架的统一。

### Spring Boot解决日志框架冲突

`pom.xml` 中书写了父类的依赖关系指向 `spring-boot-starter-parent-xxx.pom` 对 Spring Boot 框架进行配置；

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.3.4.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

`spring-boot-starter-parent-xxx.pom` 指定了 Spring Boot 中引用所有框架的依赖包 `spring-boot-dependencies.pom`；

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.3.4.RELEASE</version>
</parent>
```

`spring-boot-dependencies.pom` 中指定jar包的版本以及对框架中默认的日志框架排除；

```xml
<dependency>
    <groupId>org.apache.activemq</groupId>
    <artifactId>activemq-console</artifactId>
    <version>${activemq.version}</version>
    <exclusions>
        <exclusion>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

也可以在导入框架时排除默认的日志框架，例如：排除 Spring Boot 中默认的日志框架配置；

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <artifactId>spring-boot-starter-logging</artifactId>
            <groupId>org.springframework.boot</groupId>
        </exclusion>
    </exclusions>
</dependency>
```

Spring Boot排除其他框架中默认的日志框架时，在 `pom.xml` 文件中使用 `<exclusion></exclusion>` 标签进行依赖框架的排除。

Spring Boot中日志的配置总结：

① Spring Boot底层采用 `slf4j + logback` 的方式进行日志记录；

② Spring Boot将其他的日志框架替换成了 `slf4j` 框架；

③ 日志中间替换包实际上还是调用了 `slf4j` 的方法；

④ 当我们引入其他框架时，需要将框架中默认的日志框架移除掉；

一句话总结：Spring Boot 能自动适配所有日志，并且底层采用 `slf4j` 和 `logback` 框架作为日志记录，当我们引入其他框架时，需要将框架中的默认日志框架移除掉。

## 4. 日志的使用

### 默认的日志配置

Spring Boot默认帮我们配置好日志，可以直接使用。

- 日志的输出级别，由高到低依次为：`trace<debug < info < warn < error`；
- 日志的输出级别可以调整，设置日志输出后，日志只会在当前设置级别和以后的级别生效。
- 例如：设置了 `info` 级别的日志输出，则 `trace` 级别和 `debug` 级别的日志不会再输出；
- Spring Boot默认的日志输出级别是 `info` 级别（`root` 级别）；

代码示例：

```java
@RunWith(SpringRunner.class)
@SpringBootTest
class SpringbootLoggingApplicationTests {

	Logger logger = LoggerFactory.getLogger(getClass());

	/**
	 * 日志的输出级别：由高到低 trace<debug<info<warn<error
	 * 可以调整日志的输出级别；设置日志输出级别后，日志只会在当前级别和以后的级别生效
	 */
	@Test
	void contextLoads() {
		logger.trace("trace级别日志。。。。。");
		logger.debug("debug级别日志。。。。。");
		// Spring Boot默认输出info级别的日志，没有指定日志级别则使用Spring Boot默认的级别（root级别）
		logger.info("info级别日志。。。。。");
		logger.warn("warn级别日志。。。。。");
		logger.error("error级别日志。。。。。");
	}

}
```

运行后输出的内容：

```bash
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.3.4.RELEASE)

2020-10-20 14:46:25.049  INFO 17748 --- [           main] .b.s.Springboot03LoggingApplicationTests : Starting Springboot03LoggingApplicationTests on YOGA-S740 with PID 17748 (started by Bruce in E:\workspace\workspace_idea03\demo-springBoot\springboot03_logging)
2020-10-20 14:46:25.051  INFO 17748 --- [           main] .b.s.Springboot03LoggingApplicationTests : No active profile set, falling back to default profiles: default
2020-10-20 14:46:26.248  INFO 17748 --- [           main] o.s.s.concurrent.ThreadPoolTaskExecutor  : Initializing ExecutorService 'applicationTaskExecutor'
2020-10-20 14:46:26.520  INFO 17748 --- [           main] .b.s.Springboot03LoggingApplicationTests : Started Springboot03LoggingApplicationTests in 1.745 seconds (JVM running for 2.833)

2020-10-20 14:46:26.716  INFO 17748 --- [           main] .b.s.Springboot03LoggingApplicationTests : info级别日志。。。。。
2020-10-20 14:46:26.716  WARN 17748 --- [           main] .b.s.Springboot03LoggingApplicationTests : warn级别日志。。。。。
2020-10-20 14:46:26.716 ERROR 17748 --- [           main] .b.s.Springboot03LoggingApplicationTests : error级别日志。。。。。
```

可以通过 `application.properties` 文件中修改Spring Boot的日志默认配置，此文件也可以修改Spring Boot其他配置，详细请查看Spring Boot之配置。

① `logging.level`：指定日志的输出级别，可以为项目中的每个包和类单独配置日志的输出级别；

② `logging.file.path`：指定日志的输出路径；

③ `logging.file.name`：指定日志的输出名称和路径，当 `path` 和 `name` 属性同时配置时，path 配置不会生效；

④ `logging.pattern.console`：指定控制台输出的样式；

`logging.file.path` 和 `logging.file.file` 输出的区别：

| `logging.file.name` | `logging.file.path` |  Example   |                         Description                          |
| :-----------------: | :-----------------: | :--------: | :----------------------------------------------------------: |
|       (none)        |       (none)        |            |                    只在控制台输出日志信息                    |
|     指定文件名      |       (none)        |  `my.log`  | 指定输出的日志文件名称。名称可以是一个确切的位置或相对于当前项目目录。 |
|       (none)        |      指定目录       | `/var/log` | 输出 spring.log 日志文件到指定的目录，名称可以是一个确切的位置或相对于当前项目目录。 |

日志样式输出配置项: 

|    配置项     |                    配置内容                    |
| :-----------: | :--------------------------------------------: |
|     `%d`      |                表示日期时间配置                |
|   `%thread`   |                  表示线程名称                  |
|   %-5level    |            表示从左显示5个字符宽度             |
| `%logger{50}` | 表示logger名字最长50个字符，否则按照句点分割。 |
|     %msg      |                  表示日志消息                  |
|      %n       |                    表示换行                    |

日志输出样式示例：`%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} -%msg%n`

代码示例:

```properties
# 配置日志的输出级别，可以为每个包和类单独配置日志输出级别
logging.level.cn.bruce=trace

# path指定日志的输出目录，会在磁盘根路径下创建spring文件夹，并在目录下生成spring.log文件记录日志信息
# 不加/则在项目目录下创建文件夹，并在文件夹内生成日志输出文件spring.log
#logging.file.path=/spring
# name指定输出文件的名称和路径，文件位置可以自由指定。path和name同时书写时只有name会生效
logging.file.name=E:/spring/demo-spring.log

# 指定控制台输出的日志样式
# %d表示日期时间，
# %thread表示线程名，
# %‐5level：级别从左显示5个字符宽度
# %logger{50} 表示logger名字最长50个字符，否则按照句点分割。
# %msg：日志消息，
# %n是换行符

logging.pattern.console=%d{yyyy-MM-dd HH:mm:ss.SSS} --> [%thread] --> %-5level --> %logger{50} --> %msg%n

# 指定文件输出日志样式
logging.pattern.file=%d{yyyy-MM-dd} === [%thread] === %-5level === %logger{50} === %msg%n
```

### 指定日志配置: 

我们可以给日志框架指定配置文件，只要将配置文件放在在类路径下即可。SpringBoot就不会使用默认配置了。

不同日志框架对应的配置文件：

|     Logging System      |                        Customization                         |
| :---------------------: | :----------------------------------------------------------: |
|         Logback         | logback-spring.xml, logback-spring.groovy, logback.xml, or logback.groovy |
|         Log4j2          |               log4j2-spring.xml or log4j2.xml                |
| JDK (Java Util Logging) |                      logging.properties                      |

- `logback.xml`：配置文件会直接被日志框架识别；
- `logback-spring.xml`：日志框架不直接加载日志的配置项，由 Spring Boot 解析日志解析，可以使用 Spring Boot 的高级 Profile 功能。更多参考前文 Spring Boot 配置相关的内容。
- 注意：如果使用 `logback.xml` 仍然使用 Spring Boot 的高级 Profile 功能会报错。

Spring Boot官方文档中关于日志的配置：<a href="https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-logging">Spring Boot的日志配置</a>

## 5. 切换日志框架

Spring Boot 允许我们替换默认的 `slf4j + logback` 日志框架结构，可以切换成其他的日志配置框架。例如：我们将日志输出框架切换为 `slf4j+log4j` 的方式。

修改pom.xml文件:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <!-- 在SpringBoot框架中排除logback日志框架 -->
        <exclusion>
            <artifactId>logback-classic</artifactId>
            <groupId>ch.qos.logback</groupId>
        </exclusion>
        <!-- 在SpringBoot框架中排除logback转换框架 -->
        <exclusion>
            <artifactId>log4j-to-slf4j</artifactId>
            <groupId>org.apache.logging.log4j</groupId>
        </exclusion>
    </exclusions>
</dependency>

<!-- 引入log4j适配包，将logback框架替换为log4j框架 -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
</dependency>
```

实际开发中并不建议切换到`log4j`框架，`log4j`框架存在性能问题，建议使用`logback`或`log4j2`日志框架。

切换成 log4j2 作为日志输出框架

修改 `pom.xml` 文件

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <!-- 排除SpringBoot中logging的starter -->
        <exclusion>
            <artifactId>spring-boot-starter-logging</artifactId>
            <groupId>org.springframework.boot</groupId>
        </exclusion>
    </exclusions>
</dependency>

<!-- 导入log4j的starter -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```

切换成log4j2框架后的依赖关系

![image-20201020170741477](https://gitee.com/cao_ziqiang/img/raw/master/20210518191754.png)