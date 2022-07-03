---
title: RabbitMQ-03-使用
categories:
  - MQ
tags:
  - MQ
  - backend
  - 中间件技术
abbrlink: 6965b780
date: 2021-09-21 17:59:37
---

## 简单的生产者-消费者

对于MQ,主要的作用就是生产者发送消息和消费者接受消息

新建一个简单的$springboot$工程

```xml
<dependency>
	<groupId>com.rabbitmq</groupId>
	<artifactId>amqp-client</artifactId>
</dependency>
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>slf4j-nop</artifactId>
</dependency>
```

引入$amqp$和$slf4j-nop$的依赖,同时记得排除$springboot$中自带的$logback$。虽然没有影响，但是控制台一堆红色属实不好看。

然后就可以编写生产者和消费者的代码：

生产者：

```java
public class Sender {
    private final static String QUEUE_NAME = "hello";

    public static void main(String[] args) throws IOException, TimeoutException {
        // 创建连接工厂
        ConnectionFactory connectionFactory = new ConnectionFactory();
        // 设置rabbitmq地址
        connectionFactory.setHost("192.168.10.132");
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("homyit");
        // 建立连接
        Connection connection = connectionFactory.newConnection();
        // 获得信道
        Channel channel = connection.createChannel();
        // 声明队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        // 发布消息
        String message = "Hello World!2";
        channel.basicPublish("", QUEUE_NAME, null, message.getBytes("UTF-8"));
        System.out.println("发送了消息 : " + message);
        // 关闭连接
        channel.close();
        connection.close();
    }
}
```

消费者：

```java
public class Receiver {
    private final static String QUEUE_NAME = "hello";

    public static void main(String[] args) throws IOException, TimeoutException {
        // 创建连接工厂
        ConnectionFactory connectionFactory = new ConnectionFactory();
        // 设置rabbitmq地址
        connectionFactory.setHost("192.168.10.132");
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("homyit");
        // 建立连接
        Connection connection = connectionFactory.newConnection();
        // 获得信道
        Channel channel = connection.createChannel();
        // 声明队列
        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        // 接收消息
        channel.basicConsume(QUEUE_NAME, true, new DefaultConsumer(channel) {
            @Override
            public void handleDelivery(String consumerTag,
                                       Envelope envelope,
                                       AMQP.BasicProperties properties,
                                       byte[] body) throws IOException {
                String message = new String(body, "utf-8");
                System.out.println("收到了消息 : " + message);
            }
        });
    }
}
```

![image-20210921180438878](http://static.codenote.xyz/img/20210921180439.png)

<hr/>

![image-20210921180502928](http://static.codenote.xyz/img/20210921180503.png)

## 多个消费者

如果是同样的消息处理能力，那么MQ将会平均的分发消息给消费者。

如果压力不同，那么会根据任务的处理速度来消费消息。

MQ的默认模式为“循环调度”。

在实战场景中，还需要加入公平派遣和消息确认机制。

实现公平派遣，需要设置$basicQos$以及将$basicAck$设置为$false$。

## 协议

协议，网络协议的简称，网络协议是通信计算机双方必须共同遵从的一组约定。如怎么样建立连接、怎么样互相识别等。只有遵守这个约定，计算机之间才能相互通信交流。它的三要素是：语法、语义、时序。

为了使数据在网络上从源到达目的，网络通信的参与方必须遵循相同的规则，这套规则称为协议（protocol），它最终体现为在网络上传输的数据包的格式。

$RabbitMQ$采用的是AMQP协议。

对于大部分中间件来说，都没有直接使用HTTP协议。原因如下。

对于一个消息中间件来说，其主要责任就是负责数据传递，存储，分发，高性能和简洁才是我们所追求的，而 HTTP 请求报文头和响应报文头是比较复杂的，包含了Cookie，数据的加密解密，窗台吗，响应码等附加的功能，我们并不需要这么复杂的功能。

同时大部分情况下 HTTP 大部分都是短链接，在实际的交互过程中，一个请求到响应都很有可能会中断，中断以后就不会执行持久化，就会造成请求的丢失。这样就不利于消息中间件的业务场景，因为消息中间件可能是一个长期的获取信息的过程，出现问题和故障要对数据或消息执行持久化等，目的是为了保证消息和数据的高可靠和稳健的运行

AMQP的特点在于**天生支持分布式事务支持，可以将消息持久化，高性能以及高可用的消息处理优势**。

架构图如下：

![img](http://static.codenote.xyz/img/20210921202235.webp)

- `Producer`：消息的生产者（发送消息的程序）。
- `Connection`：应用程序与Broker之间的网络连接。
- `Channel`：信道，即信息传输的通道，可以建立多个 Channel，每个 Channel 代表一个会话任务。
	- 信道是建立在 TCP 连接内的虚拟连接，信息的读写都通过信道传输，因为对于操纵系统而言，建立和销毁 TCP 是非常昂贵的，所以引入了信道的概念，以复用一条 TCP 连接。
- `Broker(Server)` ：标识消息队列服务器实体，例如这里就是 RabbitMQ Server。
- `Virtual Host`：虚拟主机，一个 Broker 中可以设置多个 Virtual Host，用作不同用户的权限隔离。
	- Broker 可以理解为整个数据库服务，而 Virtual Host 就是其中每个数据库的感觉，不同项目可以对应不同的数据库，其中有着项目所属的业务表等等。
	- 每个 Virtual Host 中，可以有若干个 Exchange 和 Queue。
- `Exchange`：交换机，用来接收生产者发送的消息，然后将这些消息根据路由键发送到队列。
- `Binding`：Exchange 和 Queue 之间的虚拟连接，Binding 中可以包括多个 Routing key。
- ` Routing key`：路由规则，虚拟机用它来确认如何路由一个特定消息。
- `Queue`：消息队列，它是消息的容器，用来保存消息，每一条消息都能传入一个或者多个队列中，等待消费者消费，即取出这个消息。
- `Consumer`：消息的消费者（接收消息的程序）。


## 交换机

![img](http://static.codenote.xyz/img/20210921202907.webp)

从上面的工作流程可以看出，实际上有个关键的组件Exchange，因为**消息发送到RabbitMQ后首先要经过Exchange路由才能找到对应的Queue**。

实际上Exchange类型有四种，根据不同的类型工作的方式也有所不同。在HelloWord例子中，我们就使用了比较简单的**Direct Exchange**，翻译就是直连交换机。其余三种分别是：**Fanout exchange(广播)、Topic exchange(主题)、Headers exchange**。



### Direct

见文知意，直连交换机意思是此交换机需要绑定一个队列，要求**该消息与一个特定的路由键完全匹配**。简单点说就是一对一的，点对点的发送。

![img](http://static.codenote.xyz/img/20210921203157.webp)

完整的代码就是上面的HelloWord的例子，不再重复代码。

### Fanout

这种类型的交换机需要将队列绑定到交换机上。**一个发送到交换机的消息都会被转发到与该交换机绑定的所有队列上**。很像子网广播，每台子网内的主机都获得了一份复制的消息。简单点说就是发布订阅。

![img](http://static.codenote.xyz/img/20210921203220.webp)

常用于构建“发布-订阅”模式

### Topic

直接翻译的话叫做主题交换机，如果从用法上面翻译可能叫通配符交换机会更加贴切。这种交换机是使用通配符去匹配，路由到对应的队列。通配符有两种："*" 、 "#"。需要注意的是通配符前面必须要加上"."符号。

`*` 符号：有且只匹配一个词。比如 `a.*`可以匹配到"a.b"、"a.c"，但是匹配不了"a.b.c"。

`#` 符号：匹配一个或多个词。比如"rabbit.#"既可以匹配到"rabbit.a.b"、"rabbit.a"，也可以匹配到"rabbit.a.b.c"。

![img](http://static.codenote.xyz/img/20210921210905.webp)

### Headers

这种交换机用的相对没这么多。**它跟上面三种有点区别，它的路由不是用routingKey进行路由匹配，而是在匹配请求头中所带的键值进行路由**。如图所示：

![img](http://static.codenote.xyz/img/20210921213645.webp)

![img](http://static.codenote.xyz/img/20210921213649.webp)

创建队列需要设置绑定的头部信息，有两种模式：**全部匹配和部分匹配**。如上图所示，交换机会根据生产者发送过来的头部信息携带的键值去匹配队列绑定的键值，路由到对应的队列。

## SpringBoot整合

引入依赖

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

在$application.yml$文件中声明端口,用户名,密码等信息，再去编写MQ的配置类。

```yaml
spring:
  rabbitmq:
    host: 192.168.122.1 # 服务器地址
    port: 5672 # tcp端口
    username: admin # 用户名
    password: admin # 用户密码
    virtual-host: /rabbitmq_springboot_01 # 虚拟主机
```

在配置类中定义交换机，队列和绑定

```java
@Configuration
public class RabbitMqConfiguration {
    
    public static final String TOPIC_EXCHANGE = "topic_order_exchange";
    public static final String TOPIC_QUEUE_NAME_1 = "test_topic_queue_1";
    public static final String TOPIC_QUEUE_NAME_2 = "test_topic_queue_2";
    public static final String TOPIC_ROUTINGKEY_1 = "test.*";
    public static final String TOPIC_ROUTINGKEY_2 = "test.#";

    @Bean
    public TopicExchange topicExchange() {
        return new TopicExchange(TOPIC_EXCHANGE);
    }

    @Bean
    public Queue topicQueue1() {
        return new Queue(TOPIC_QUEUE_NAME_1);
    }

    @Bean
    public Queue topicQueue2() {
        return new Queue(TOPIC_QUEUE_NAME_2);
    }

    @Bean
    public Binding bindingTopic1(){
        return BindingBuilder.bind(topicQueue1())
                .to(topicExchange())
                .with(TOPIC_ROUTINGKEY_1);
    }
    @Bean
    public Binding bindingTopic2(){
        return BindingBuilder.bind(topicQueue2())
                .to(topicExchange())
                .with(TOPIC_ROUTINGKEY_2);
    }

}
```

**添加 @Configuration 注解**：表明这是一个配置类

**定义常量**：将交换机名，队列名，路由key 等都可以创建为常量，调用，管理和修改都非常方便，还可以创建出一个专门的 RabbitMQ 的常量类。

**定义交换机**：因为这个例子是 Topic 所以选择 TopicExchange 类型

**定义队列**：传入队列名常量即可，因为持久化等存在默认值，也可以自己自定持久化，是否独占等参数

**绑定交换机和队列**：利用 BindingBuilder 的 bind 方法绑定队列，to 绑定到指定交换机，with 传入路由key

生产者：

```java
@SpringBootTest(classes = RabbitmqSpringbootApplication.class)
@RunWith(SpringRunner.class)
public class RabbitMqTest {
    /**
     * 注入 RabbitTemplate
     */
    @Autowired RabbitTemplate rabbitTemplate
	private 
    @Test
    public void testTopicSendMessage() {
        rabbitTemplate.convertAndSend(RabbitMqConfiguration.TOPIC_EXCHANGE, "test.order.insert", "This is a message !");
    }
}
```

消费者

```java
@Component
public class TopicConsumer {
	// 绑定队列即可
    @RabbitListener(queues = {RabbitMqConfiguration.TOPIC_QUEUE_NAME_1})
    public void receiveMessage1(String message) {
        System.out.println("消费者1：" + message);
    }
	
    // 绑定队列即可
    @RabbitListener(queues = {RabbitMqConfiguration.TOPIC_QUEUE_NAME_2})
    public void receiveMessage2(String message) {
        System.out.println("消费者2：" + message);
    }
}
```

