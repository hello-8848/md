# Rabbitmq高级特性

## 学习目标

+ 掌握常见的高级特性
+ 高级特性生产者可靠性消息投递
+ 高级特性消费者ACK确认机制
+ 理解相关应用性的解决方案
+ 了解相关集群的搭建



## 1 RabbitMq高级特性

在消息的使用过程当中存在一些问题。比如发送消息我们如何确保消息的投递的可靠性呢？如何保证消费消息可靠性呢？如果不能保证在某些情况下可能会出现损失。比如当我们发送消息的时候和接收消息的时候能否根据消息的特性来实现某一些业务场景的模拟呢？订单30分钟过期等等，系统通信的确认等等。

### 1.1 生产者可靠性消息投递

可靠性消息

在使用 RabbitMQ 的时候，作为消息发送方希望杜绝任何消息丢失或者投递失败场景。RabbitMQ 为我们提供了两种方式用来控制消息的投递可靠性模式，mq提供了如下两种模式：

```
+ confirm模式
	生产者发送消息到交换机的时机 
+ return模式
    交换机转发消息给queue的时机
```



MQ投递消息的流程如下：



![1583026649199](images/1583026649199.png)

```
1.生产者发送消息到交换机
2.交换机根据routingkey 转发消息给队列
3.消费者监控队列，获取队列中信息
4.消费成功删除队列中的消息
```

- 消息从 product 到 exchange 则会返回一个 confirmCallback 。
- 消息从 exchange 到 queue 投递失败则会返回一个 returnCallback 。



#### 1.1.1 confirmcallback代码实现

执行的步骤：

```
1.创建springboot工程
2.添加起步依赖
3.设置configrm回调函数
4.发送消息
```

（1）创建springboot工程

![1583027180169](images/1583027180169.png)

（2）添加依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.itheima</groupId>
    <artifactId>itheima-springboot-rabbitmq-demo01</artifactId>
    <version>1.0-SNAPSHOT</version>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.4.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-amqp</artifactId>
        </dependency>      
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>


        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>
    </dependencies>
</project>
```

(3)在com.itheima下创建启动类,并创建配置交换机队列和绑定

```java
package com.itheima;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class RabbitmqDemo01Application {
    public static void main(String[] args) {
        SpringApplication.run(RabbitmqDemo01Application.class,args);
    }
      //创建队列
    @Bean
    public Queue createqueue(){
        return new Queue("queue_demo01");
    }

    //创建交换机

    @Bean
    public DirectExchange createExchange(){
        return new DirectExchange("exchange_direct_demo01");
    }

    //创建绑定
    @Bean
    public Binding createBinding(){
        return BindingBuilder.bind(createqueue()).to(createExchange()).with("item.insert");
    }
}
```



（4）创建application.yml文件，配置如下，配置开启confirms模式，默认为false

```yaml
spring:
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest
    publisher-confirms: true
server:
  port: 8080
```



(5)创建controller 发送消息



```java
@RestController
@RequestMapping("/test")
public class TestController {

    @Autowired
    private RabbitTemplate rabbitTemplate;

    @Autowired
    private RabbitTemplate.ConfirmCallback myConfirmCallback;

    /**
     * 发送消息
     *
     * @return
     */
    @RequestMapping("/send1")
    public String send1() {
        //设置回调函数
        rabbitTemplate.setConfirmCallback(myConfirmCallback);
        //发送消息
        rabbitTemplate.convertAndSend("exchange_direct_demo01", "item.insert", "hello insert");
        return "ok";
    }
}
```

(6)创建回调函数

```java
package com.itheima.confirm;

import org.springframework.amqp.rabbit.connection.CorrelationData;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.lang.Nullable;
import org.springframework.stereotype.Component;

@Component
public class MyConfirmCallback implements RabbitTemplate.ConfirmCallback {

    /**
     *
     * @param correlationData 消息信息
     * @param ack  确认标识：true,MQ服务器exchange表示已经确认收到消息 false 表示没有收到消息
     * @param cause  如果没有收到消息，则指定为MQ服务器exchange消息没有收到的原因，如果已经收到则指定为null
     */
    @Override
    public void confirm(@Nullable CorrelationData correlationData, boolean ack, @Nullable String cause) {
        if(ack){
            System.out.println("发送消息到交换机成功,"+cause);
        }else{
            System.out.println("发送消息到交换机失败,原因是："+cause);
        }
    }
}
```

(7)测试发送消息：

启动应用，浏览器发送请求：`<http://localhost:8080/test/send1>`

打印如下：

![1583028306804](images/1583028306804.png)

稍微注意下：此时我们没有监听消息，那么只表示发送消息到交换机成功。



此时如果我们把交换机名称换掉，会出现失败的案例，如下：

![1583028417504](images/1583028417504.png)

再次重启测试如下：

![1583028450398](images/1583028450398.png)



总结：

```
1.发送放可以根据confrim机制来确保是否消息已经发送到交换机
2.confirm机制能保证消息发送到交换机有回调，不能保证消息转发到queue有回调
```



#### 1.1.2 returncallback代码实现

如上，已经实现了消息发送到交换机上的内容，但是如果是，交换机发送成功，但是在路由转发到队列的时候，发送错误，此时就需要用到returncallback模式了。接下来我们实现下。

实现步骤如下：

```
1.开启returncallback模式
2.设置回调函数
3.发送消息
```



（1）配置yml开启returncallback

![1583028698117](images/1583028698117.png)



(2)编写returncallback代码：

```java
@Component
public class MyReturnCallBack implements RabbitTemplate.ReturnCallback {
    /**
     *
     * @param message 消息信息
     * @param replyCode 退回的状态码
     * @param replyText 退回的信息
     * @param exchange 交换机
     * @param routingKey 路由key
     */
    @Override
    public void returnedMessage(Message message, int replyCode, String replyText, String exchange, String routingKey) {
        System.out.println("退回的消息是："+new String(message.getBody()));
        System.out.println("退回的replyCode是："+replyCode);
        System.out.println("退回的replyText是："+replyText);
        System.out.println("退回的exchange是："+exchange);
        System.out.println("退回的routingKey是："+routingKey);

    }
}
```

(3)发送消息

我们发送正确的交换机 ，但是发送错误的routingkey测试下



![1583029283733](images/1583029283733.png)



```java
@Autowired
private RabbitTemplate.ReturnCallback myReturnCallback;


@RequestMapping("/send2")
public String send2() {
    //设置回调函数
    rabbitTemplate.setReturnCallback(myReturnCallback);
    //发送消息
    rabbitTemplate.convertAndSend("exchange_direct_demo01", "item.insert1234", "hello insert");
    return "ok";
}
```



（4）测试发送请求出现如下，说明测试成功。

![1583029392253](images/1583029392253.png)

如果我们两个都设置了那么就变成这样：

![1583029510203](images/1583029510203.png)



（5）小结

```
+ returncallback模式，需要手动设置开启
+ 该模式 指定 在路由的时候发送错误的时候调用回调函数，不影响消息发送到交换机
```



#### 1.1.2 两种模式的总结

confirm模式用于在消息发送到交换机时机使用，return模式用于在消息被交换机路由到队列中发送错误时使用。

但是一般情况下我们使用confirm即可，因为路由key 由开发人员指定，一般不会出现错误。如果要保证消息在交换机和routingkey的时候那么需要结合两者的方式来进行设置。



### 1.2 消费者确认机制（ACK）

上边我们学习了发送方的可靠性投递，但是在消费方也有可能出现问题，比如没有接受消息，比如接受到消息之后，在代码执行过程中出现了异常，这种情况下我们需要额外的处理，那么就需要手动进行确认签收消息。rabbtimq给我们提供了一个机制：ACK机制。

ACK机制：有三种方式

+ 自动确认  acknowledge="**none**"
+ 手动确认  acknowledge="**manual**"
+ 根据异常情况来确认（暂时不怎么用） acknowledge="**auto**"



解释：

```properties
其中自动确认是指:
	当消息一旦被Consumer接收到，则自动确认收到，并将相应 message 从 RabbitMQ 的消息缓存中移除。但是在实际业务处理中，很可能消息接收到，业务处理出现异常，那么该消息就会丢失。
其中手动确认方式是指：
	则需要在业务处理成功后，调用channel.basicAck()，手动签收，如果出现异常，则调用channel.basicNack()等方法，让其按照业务功能进行处理，比如：重新发送，比如拒绝签收进入死信队列等等。
```



#### 1.2.1 ACK代码实现

实现的步骤：

```
1.创建普通消息这监听器监听消息
2.修改controller 发送正确消息测试
3.设置配置文件开启ack手动确认，默认是自动确认
4.修改消息监听器进行手动确认业务判断逻辑
```



（1）创建普通消息监听器

```java
package com.itheima.listener;

import org.springframework.amqp.rabbit.annotation.RabbitHandler;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

@Component
@RabbitListener(queues = "queue_demo01")
public class MyRabbitListener {

    @RabbitHandler
    public void msg(String message) {
        System.out.println("消费Duang接收消息：" + message);
    }
}
```



(2)修改Testcontroller方法用于测试发送正确消息：

```java
/**
 * 发送正确消息
 * @return
 */
@RequestMapping("/send3")
public String send3() {
    //设置回调函数
    //发送消息
    rabbitTemplate.convertAndSend("exchange_direct_demo01", "item.insert", "hello insert");
    return "ok";
}
```



测试OK，如下图：

![1583030565551](images/1583030565551.png)



（3）设置yml设置为手动确认模式

![1583030785798](images/1583030785798.png)

（4）修改监听类

如下，此时我们并没有手动签收，就是不签收消息

```java
@Component
@RabbitListener(queues = "queue_demo01")
public class MyRabbitListener {

    /*@RabbitHandler
    public void msg(String message) {
        System.out.println("消费Duang接收消息：" + message);
    }*/
    @RabbitHandler
    public void msg(Message message, Channel channel ,String msg) {
        System.out.println("消费Duang接收消息：" + msg);
    }
}
```

(5)测试：

发送消息之后，队列中出现：

![1583031037831](images/1583031037831.png)

控制台打印：

![1583031069445](images/1583031069445.png)

说明一直没有被签收。

#### 1.2.2 ACK确认的方式

ack确认方式有几种：

+ 签收消息
+ 拒绝消息  批量处理/单个处理==将消息放回队列,丢弃消息

以上可以根据不同的业务进行不同的选择。需要注意的是，如果拒绝签收，下一次启动又会自动的进行消费。



监听代码的业务实现步骤：

```java
//接收消息
//处理本地业务
//签收消息
//如果出现异常，则拒绝消息 可以重回队列 也可以丢弃 可以根据业务场景来
```

```java
@RabbitHandler
public void msg(Message message, Channel channel, String msg) {
    //接收消息
    System.out.println("消费Duang接收消息：" + msg);
    try {
        //处理本地业务
        System.out.println("处理本地业务开始======start======");
        Thread.sleep(2000);
        System.out.println("处理本地业务结束======end======");
        //签收消息
       
    } catch (Exception e) {
        e.printStackTrace();
        //如果出现异常，则拒绝消息 可以重回队列 也可以丢弃 可以根据业务场景来
    }

}
```



```java
第一种：签收
channel.basicAck()
第二种：拒绝签收 批量处理 
channel.basicNack() 
第三种：拒绝签收 不批量处理
channel.basicReject()
```



正常则签收，不正常则进行丢弃处理。

```java
@RabbitHandler
public void msg(Message message, Channel channel, String msg) {
    //接收消息
    System.out.println("消费Duang接收消息：" + msg);
    try {
        //处理本地业务
        System.out.println("处理本地业务开始======start======");
        Thread.sleep(2000);
        int i=1/0;
        System.out.println("处理本地业务结束======end======");
        //签收消息
        channel.basicAck(message.getMessageProperties().getDeliveryTag(), false);
    } catch (Exception e) {
        e.printStackTrace();
        //如果出现异常，则拒绝消息 可以重回队列 也可以丢弃 可以根据业务场景来
        try {
            channel.basicNack(message.getMessageProperties().getDeliveryTag(),false,false);
            //channel.basicReject(message.getMessageProperties().getDeliveryTag(),false);
        } catch (Exception e1) {
            e1.printStackTrace();
        }
    }

}
```



消息丢弃，则没有消息存在。

![1583032150778](images/1583032150778.png)





正常则签收，不正常再重回队列进行再次投递：

channel.basicNack（,,true）第三个参数设置为重回队列进行再次投递。

```java
@RabbitHandler
public void msg(Message message, Channel channel, String msg) {
    //接收消息
    System.out.println("消费Duang接收消息：" + msg);
    try {
        //处理本地业务
        System.out.println("处理本地业务开始======start======");
        Thread.sleep(2000);
        int i = 1 / 0;
        System.out.println("处理本地业务结束======end======");
        //签收消息
        channel.basicAck(message.getMessageProperties().getDeliveryTag(), false);
    } catch (Exception e) {
        e.printStackTrace();
        //如果出现异常，则拒绝消息 可以重回队列 也可以丢弃 可以根据业务场景来
        try {
            if (message.getMessageProperties().getRedelivered()) {
                //消息已经重新投递，不需要再次投递
                System.out.println("已经投递一次了");
            } else {
                //第三个参数：设置是否重回队列     
                channel.basicNack(message.getMessageProperties().getDeliveryTag(), false, true);
            }

            //channel.basicReject(message.getMessageProperties().getDeliveryTag(),false);
        } catch (Exception e1) {
            e1.printStackTrace();
        }
    }

}
```

如图：还有没确认的消息，下次继续可以消费。

![1583032677601](images/1583032677601.png)



#### 1.2.3 小结

- 设置acknowledge属性，设置ack方式 none：自动确认，manual：手动确认
- 如果在消费端没有出现异常，则调用channel.basicAck(deliveryTag,false);方法确认签收消息
- 如果出现异常，则在catch中调用 basicNack或 basicReject，拒绝消息，让MQ重新发送消息。

```properties
如何保证消息的高可靠性传输？

1.持久化

• exchange要持久化

• queue要持久化

• message要持久化

2.生产方确认Confirm、Return

3.消费方确认Ack
```



### 1.3 消费端限流

#### 1.3.1 消费端限流说明

如果并发量大的情况下，生产方不停的发送消息，可能处理不了那么多消息，此时消息在队列中堆积很多，当消费端启动，瞬间就会涌入很多消息，消费端有可能瞬间垮掉，这时我们可以在消费端进行限流操作，每秒钟放行多少个消息。这样就可以进行并发量的控制，减轻系统的负载，提供系统的可用性，这种效果往往可以在秒杀和抢购中进行使用。在rabbitmq中也有限流的一些配置。

![1583033190603](images/1583033190603.png)

吞吐量: 吞 和 吐

#### 1.3.2 代码实现测试

实现步骤：

```java
1.设置限流的量
2.进行测试即可
```

配置如下：

![1583035714321](images/1583035714321.png)

默认是250个。

消费端代码，模拟每隔一秒钟处理一个消息

![1583035793018](images/1583035793018.png)



测试：并发发送10个消息，此时，如下图所示，每一个都是一个处理一个只有等处理完成之后，才能继续处理。

![1583035756613](images/1583035756613.png)



### 1.4 TTL

TTL 全称 Time To Live（存活时间/过期时间）。当消息到达存活时间后，还没有被消费，会被自动清除。

RabbitMQ设置过期时间有两种： 10分钟

+ 针对某一个队列设置过期时间 ；队列中的所有消息在过期时间到之后，如果没有被消费则被全部清除
+ 针对某一个特定的消息设置过期时间；队列中的消息设置过期时间之后，如果这个消息没有被消息则被清除。30分钟

```properties
需要注意一点的是：
 	针对某一个特定的消息设置过期时间时，一定是消息在队列中在队头的时候进行计算，如果某一个消息A 设置过期时间5秒，消息B在队头，消息B没有设置过期时间，B此时过了已经5秒钟了还没被消费。注意，此时A消息并不会被删除，因为它并没有再队头。

一般在工作当中，单独使用TTL的情况较少。我们后面会讲到延时队列。在这里有用处。
```



演示TTL 代码步骤：

```
1.创建配置类配置 过期队列 交换机 和绑定
2.创建controller 测试发送消息
```

（1）创建配置类：

```java
package com.itheima.config;

import org.springframework.amqp.core.*;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;


@Configuration
public class TtlConfig {

    //创建过期队列
    @Bean
    public Queue createqueuettl1(){
        //设置队列过期时间为10000 10S钟
        return QueueBuilder.durable("queue_demo02").withArgument("x-message-ttl",10000).build();
    }

    //创建交换机

    @Bean
    public DirectExchange createExchangettl(){
        return new DirectExchange("exchange_direct_demo02");
    }

    //创建绑定
    @Bean
    public Binding createBindingttl(){
        return BindingBuilder.bind(createqueuettl1()).to(createExchangettl()).with("item.ttl");
    }
}
```

(2)创建controller测试

```
   /**
     * 发送 ttl测试相关的消息
     * @return
     */
    @RequestMapping("/send4")
    public String send4() {
        //设置回调函数
        //发送消息
        rabbitTemplate.convertAndSend("exchange_direct_demo02", "item.ttl", "hello ttl哈哈");
        return "ok";
    }
}
```



(3)测试

过10S钟之后，该数据就都被清0

![1583038092684](images/1583038092684.png)







### 1.5 死信队列

#### 1.5.1 死信队列的介绍

​	死信队列：当消息成为Dead message后，可以被重新发送到另一个交换机，这个交换机就是Dead Letter Exchange（死信交换机 简写：DLX）。

如下图的过程：

![1583045913151](images/1583045913151.png)

成为死信的三种条件：

1. 队列消息长度到达限制；
2. 消费者拒接消费消息，basicNack/basicReject,并且不把消息重新放入原目标队列,requeue=false；
3. 原队列存在消息过期设置，消息到达超时时间未被消费；

#### 1.5.2 死信的处理过程



DLX也是一个正常的Exchange，和一般的Exchange没有区别，它能在任何的队列上被指定，实际上就是设置某个队列的属性。

当这个队列中有死信时，RabbitMQ就会自动的将这个消息重新发布到设置的Exchange上去，进而被路由到另一个队列。

可以监听这个队列中的消息做相应的处理。



#### 1.5.3 死信队列的设置

刚才说到死信队列也是一个正常的exchange.只需要设置一些参数即可。

给队列设置参数： x-dead-letter-exchange 和 x-dead-letter-routing-key。

如上图所示：

```properties
1.创建queue1 正常队列  用于接收死信队列过期之后转发过来的消息
2.创建queue2 可以针对他进行参数设置 死信队列
3.创建交换机  死信交换机
4.绑定正常队列到交换机 
```



(1)创建配置类用于配置死信队列 死信交换机 死信路由 和正常队列

```java
package com.itheima.config;

import org.springframework.amqp.core.*;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class DlxConfig {


    //正常的队列    接收死信队列转移过来的消息
    @Bean
    public Queue createqueuetdlq(){
        return QueueBuilder.durable("queue_demo03").build();
    }

    //死信队列   --->将来消息发送到这里
    @Bean
    public Queue createqueuetdelq2(){
        return QueueBuilder
            .durable("queue_demo03_deq")
            .withArgument("x-message-ttl",10000)//设置队列的消息过期时间
            .withArgument("x-dead-letter-exchange","exchange_direct_demo03_dlx")//设置死信交换机
            .withArgument("x-dead-letter-routing-key","item.dlx")//设置死信路由key
            .build();
    }

    //创建交换机

    @Bean
    public DirectExchange createExchangedel(){
        return new DirectExchange("exchange_direct_demo03_dlx");
    }

    //创建绑定 将正常队列绑定到死信交换机上
    @Bean
    public Binding createBindingdel(){
        return BindingBuilder.bind(createqueuetdlq()).to(createExchangedel()).with("item.dlx");
    }


}
```

(2)添加controller的方法用于测试

```java
/**
 * 测试发送死信队列
 * @return
 */
@RequestMapping("/send5")
public String send5() {
    //发送消息到死信队列   可以使用默认的交换机 指定ourtingkey为死信队列名即可

    rabbitTemplate.convertAndSend("queue_demo03_deq", "hello dlx哈哈");
    return "ok";
}
```



##### 1.5.3.1 测试超时进入死信

测试：

浏览器中输入：

`<http://localhost:8080/test/send5>`



查看控制台：

​	队列数据为1

![1583048652675](images/1583048652675.png)



经过10S钟之后： 变成0 由此上边的demo03正常队列中多了一个消息。

![1583048680153](images/1583048680153.png)



至此，我们测试了过期进入死信队列。



##### 1.5.3.2 测试拒绝签收进入死信

（1）创建监听器

```java
@Component
@RabbitListener(queues = "queue_demo03_deq")
public class DLxListner {
    @RabbitHandler
    public void lis(Message message, Channel channel,String msg){
        System.out.println("消息是:"+msg);
        try {
            System.out.println("我拒绝签收");
            channel.basicNack(message.getMessageProperties().getDeliveryTag(),false,false);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```



(2)发送消息测试如下：

![1583049078968](images/1583049078968.png)

控制台由原来的3立即变成4 不需要等待10S

![1583049090490](images/1583049090490.png)



##### 1.5.3.3 测试设置长度进入死信

（1） 修改配置，添加队列长度参数

![1583049428181](images/1583049428181.png)

（2）再控制台中删除交换机 队列 重新启动系统，才能生效。



（3）测试：

点击发送4次消息，如图立即有3条进入死信，还有一条在队列中，等10S钟之后，也会进入死信。

![1583049340454](images/1583049340454.png)



10S钟之后：

![1583049390226](images/1583049390226.png)



### 1.6 延迟队列

​	延迟队列，即消息进入队列后不会立即被消费，只有到达指定时间后，才会被消费。在rabbitmq中，并没有延迟队列概念，但是我们可以使用ttl 和死信队列的方式进行达到延迟的效果。这种需求往往在某些应用场景中出现。当然还可以使用插件。

如图所示：

![1583049753799](images/1583049753799.png)

```
1.生产者产生一个消息发送到queue1
2.queue1中的消息过期则转发到queue2
3.消费者在queue2中获取消息进行消费
```



如上场景中 典型的案例：下订单之后，30分钟如果还未支付则，取消订单回滚库存。我们来模拟下需求：

（1）创建配置类

```java
@Configuration
public class DelayConfig {
    //正常的队列    接收死信队列转移过来的消息
    @Bean
    public Queue createQueue2(){
        return QueueBuilder.durable("queue_order_queue2").build();
    }

    //死信队列   --->将来消息发送到这里  这里不设置过期时间，我们应该在发送消息时设置某一个消息（某一个用户下单的）的过期时间
    @Bean
    public Queue createQueue1(){
        return QueueBuilder
                .durable("queue_order_queue1")
                .withArgument("x-dead-letter-exchange","exchange_order_delay")//设置死信交换机
                .withArgument("x-dead-letter-routing-key","item.order")//设置死信路由key
                .build();
    }

    //创建交换机
    @Bean
    public DirectExchange createOrderExchangeDelay(){
        return new DirectExchange("exchange_order_delay");
    }

    //创建绑定 将正常队列绑定到死信交换机上
    @Bean
    public Binding createBindingDelay(){
        return BindingBuilder.bind(createQueue2()).to(createOrderExchangeDelay()).with("item.order");
    }
}
```



(2)修改controller

![1583050590245](images/1583050590245.png)



注意：发送消息要发送到queue1,监听消息要监听queue2



```java
/**
 * 发送下单
 *
 * @return
 */
@RequestMapping("/send6")
public String send6() {
    //发送消息到死信队列   可以使用默认的交换机 指定ourtingkey为死信队列名即可
    System.out.println("用户下单成功，10秒钟之后如果没有支付，则过期，回滚订单");
    System.out.println("时间："+new Date());
    rabbitTemplate.convertAndSend("queue_order_queue1", (Object) "哈哈我要检查你是否有支付", new MessagePostProcessor() {
        @Override
        public Message postProcessMessage(Message message) throws AmqpException {
            message.getMessageProperties().setExpiration("10000");//设置该消息的过期时间
            return message;
        }
    });
    return "用户下单成功，10秒钟之后如果没有支付，则过期，回滚订单";
}
```



（3）设置监听类

注意，监听消息要监听queue2 ，发送消息要发送queue1

```java
@Component
@RabbitListener(queues = "queue_order_queue2")
public class OrderListener {

    @RabbitHandler
    public void orderhandler(Message message, Channel channel, String msg) {
        System.out.println("获取到消息:" + msg + ":时间为:" + new Date());
        try {
            System.out.println("模拟检查开始=====start");
            Thread.sleep(1000);
            System.out.println("模拟检查结束=====end");
            System.out.println("用户没付款，检查没通过，进入回滚库存处理");
            channel.basicAck(message.getMessageProperties().getDeliveryTag(), false);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```



(4)测试,浏览器发送路径：`<http://localhost:8080/test/send6>`

![1583050903952](images/1583050903952.png)



测试如下图

![1583050873000](images/1583050873000.png)

## 2 rabbitmq应用的问题:消息重复消费

幂等性指一次和多次请求某一个资源，对于资源本身应该具有同样的结果。也就是说，其任意多次执行对资源本身所产生的影响均与一次执行的影响相同。在MQ中指，消费多条相同的消息，得到与消费该消息一次相同的结果。

![1583051056646](images/1583051056646.png)

```
以转账为例：
1.发送消息 
2.消息内容包含了id 和 版本和 金额
3.消费者接收到消息，则根据ID 和版本执行sql语句，
update account set money=money-?,version=version+1 where id=? and version=?
4.如果消费第二次，那么同一个消息内容是修改不成功的。
```



## 3.RabbitMQ集群(了解)

​	实际生产应用中都会采用消息队列的集群方案，如果选择RabbitMQ那么有必要了解下它的集群方案一般来说，如果只是为了学习RabbitMQ或者验证业务工程的正确性那么在本地环境或者测试环境上使用其单实例部署就可以了，但是出于MQ中间件本身的可靠性、并发性、吞吐量和消息堆积能力等问题的考虑，在生产环境上一般都会考虑使用RabbitMQ的集群方案。



### 3.1 rabbitmq集群通信原理

​	RabbitMQ这款消息队列中间件产品本身是基于Erlang编写，Erlang语言天生具备分布式特性（通过同步Erlang集群各节点的magic cookie来实现）。因此，RabbitMQ天然支持Clustering。集群是保证可靠性的一种方式，同时可以通过水平扩展以达到增加消息吞吐量能力的目的，这里只需要保证erlang_cookie的参数一致集群即可通信。

rabbimtq集群包括两种：普通集群和镜像集群。

普通集群有缺点也有优点，镜像集群有缺点也有优点。

大致上，

如果是普通集群：那么每一个节点的数据，存储了另外一个节点的元数据，当需要使用消息时候，从另外一台节点	拉取数据，这样性能很高，但是性能瓶颈发生在单台服务器上。而且宕机有可能出现消息丢失。

如果镜像集群，那么在使用时候，每个节点都相互通信互为备份，数据共享。那么这样一来使用消息时候，就直接获取，不再零时获取，但是缺点就是消耗很大的性能和带宽。



### 3.2 rabbitmq集群搭建

​	rabbitmq集群搭建，这里我们采用docker的方式来进行搭建，由于docker还没学习，那么我们了解即可。

准备一个虚拟机 里面安装docker引擎。这里为了测试我们采用2台rabbitmq的实例,也就是两个docker容器来模拟2个rabbitmq服务器器。

准备一个虚拟机 里面安装docker引擎。这里为了测试我们采用2台rabbitmq的实例，也就是两个docker容器来模拟2个rabbitmq服务器器。

- 准备一台虚拟机 我的机器ip为192.168.211.128 .也可以使用畅购的虚拟机。

![1583481988657](images/1583481988657.png)



- 安装docker引擎

这个不再演示



##### 3.2.1 拉取镜像

执行命令：

```shell
docker pull rabbitmq:3.6.15-management
```



##### 3.2.2 创建rabbitmq容器



- 创建rabbitmq容器1：

```shell
docker run -d --hostname rabbit1 --name myrabbit1 -p 15672:15672 -p 5672:5672 -e RABBITMQ_ERLANG_COOKIE='rabbitcookie' rabbitmq:3.6.15-management
```



- 创建rabbitmq容器2：

```
docker run -d --hostname rabbit2 --name myrabbit2 -p 15673:15672 -p 5673:5672 --link=myrabbit1:rabbit1 -e RABBITMQ_ERLANG_COOKIE='rabbitcookie' rabbitmq:3.6.15-management

```







解释：

```
--link <name or id>:alias
其中，name和id是源容器的name和id，alias是源容器在link下的别名。

--link  用于在容器中进行通信的时候需要使用到的。


-e RABBITMQ_ERLANG_COOKIE='rabbitcookie'
其中 -e 设置环境变量  变量名为：RABBITMQ_ERLANG_COOKIE  值为：rabbitcookie  该值可以任意。 但是一定要注意，两个容器的cookie值一定要一样才行。他的作用用于发现不同的节点，并通过该cookie进行自动校验和通信使用。

--hostname rabbit2  
其中：--hostname 用于设置容器内部的hostname名称，如果不设置，那就会自动随机生成一个hostname字，如下图。
这里一定要设置。因为rabbitmq的节点数据进行通信加入集群的时候需要用hostname作为集群名称。


```

![1583482124822](images/1583482124822.png)





### 3.4 配置rabbitmq集群

这里我们使用 集群名 rabbit@rabbit1 ,将节点2 加入到节点1号中。

##### 3.4.1 配置rabbit1

- 进入到myrabbit1容器内部

```shell
 docker exec -it myrabbit1 bash
```



- 配置节点

```shell
rabbitmqctl stop_app
rabbitmqctl reset
rabbitmqctl start_app
exit
```



解释：

```
rabbitmqctl stop_app  --- 表示关闭节点
rabbitmqctl reset     --- 重新设置节点配置
rabbitmqctl start_app --- 重新启动 （此处不需要设置 ，将该节点作为集群master,其他节点加入到该节点中）
exit ---退出容器

```





##### 3.4.2 配置rabbitmq2 

- 进入到myrabbit2容器内部

```shell
 docker exec -it myrabbit2 bash
```



- 配置节点

```shell
rabbitmqctl stop_app
rabbitmqctl reset
rabbitmqctl join_cluster --ram rabbit@rabbit1
rabbitmqctl start_app
exit
```





解释：

```
rabbitmqctl join_cluster --ram rabbit@rabbit1

--  用于将该节点加入到集群中  
--  ram   设置为内存存储，默认为 disc 磁盘存储，如果为磁盘存储可以不用配置ram
-- rabbit@rabbit1   该 配置 为节点集群名称：集群名称为：rabbit@server  而server指定就是hostname的名称。

```





配置完成，打开web管理界面，如下图所示：

![1583482184496](images/1583482184496.png)





### 3.5 配置镜像队列(可选)

如上，我们已经搭建好了集群，但是并不能做到高可用，所以需要配置升级为镜像队列。



在任意的节点（A或者B）中执行如下命令：

```
rabbitmqctl set_policy ha-all "^" '{"ha-mode":"all"}'

```

```
解释
rabbitmqctl set_policy 
	用于设置策略
ha-all 
	表示设置为镜像队列并策略为所有节点可用 ，意味着 队列会被（同步）到所有的节点，当一个节点被加入到集群中时，也会同步到新的节点中，此策略比较保守，性能相对低，对接使用半数原则方式设置（N/2+1），例如：有3个结点 此时可以设置为：ha-two 表示同步到2个结点即可。
"^"  表示针对的队列的名称的正则表达式，此处表示匹配所有的队列名称
'{"ha-mode":"all"}' 设置一组key/value的JSON 设置为高可用模式 匹配所有exchange

```



此时查看web管理界面：添加一个队列itcast111,如下图已经可以出现结果为有一个结点，并且是ha-all模式（镜像队列模式）

![1583482207626](images/1583482207626.png)



课后要求:

1.完成rabbitmq第一天的代码;(必须)

2.完成confirm模式和return模式实现消息的可靠性投递(必须完成)

3.完成消息的ack确认机制,消息的签收 消息的拒绝(单个拒绝 批量拒绝)(必须完成)

4.消费端限流配置一下就可以

5.实现队列超时,生成死信, 长度过长生成死信,消息超时生成死信,拒绝签收且不放回队列生成死信

6.死信队列

7.延迟队列(必须完成)







