# 第2天 SpringCloud

## 学习目标

- ==能够使用Feign进行远程调用==

  ```properties
  1.feign的使用->解决远程请求中硬编码问题
  2.负载均衡配置 
  3.支持熔断配置
  4.请求/响应压缩压缩nginx
  5.日志配置
  ```
  
- ==能够搭建Spring Cloud Gateway==

  ```properties
  1.微服务网关
  2.路由
  3.过滤配置
  ```

- 能够配置Spring Cloud Gateway过滤器

- 能够使用Spring Cloud Gateway默认过滤器：==全局过滤==、局部过滤

- 能够搭建Spring Cloud Config配置中心服务

  ```properties
  1.集中管理配置文件
  ```

- 能够使用Spring Cloud Bus 消息总线实时更新配置文件

  ```properties
  .1.每个微服务的通知消息管理服务
  ```

  



## 1 Spring Cloud Feign

### 1.1 目标

- 了解Feign的作用
- 掌握Feign的使用过程
- 掌握Feign的负载均衡配置
- 掌握Feign的熔断配置
- 了解Feign的压缩配置
- 了解Feign的日志配置

### 1.2 讲解

#### 1.2.1 Feign简介 

Feign [feɪn] 译文 伪装。Feign是一个声明式WebService客户端.使用Feign能让编写WebService客户端更加简单,它的使用方法是定义一个接口，然后在上面添加注解。不再需要拼接URL，参数等操作。项目主页：https://github.com/OpenFeign/feign 。

- 集成Ribbon的负载均衡功能
- 集成了Hystrix的熔断器功能
- 支持请求压缩
- 大大简化了远程调用的代码，同时功能还增强啦
- Feign以更加优雅的方式编写远程调用代码，并简化重复代码



#### 1.2.2 快速入门

使用Feign替代RestTemplate发送Rest请求。

**实现步骤：**

```properties
1. 导入feign依赖
2. 编写Feign客户端接口
3. 消费者启动引导类开启Feign功能注解
4. 访问接口测试
```



**实现过程：**

(1)导入依赖

在`user-consumer`中添加`spring-cloud-starter-openfeign`依赖

```xml
<!--配置feign-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency> 
```



(2)创建Feign客户端

在`user-consumer`中创建`com.itheima.feign.UserClient`接口，代码如下：

![1563260831222](images\1563260831222.png)



解释：

```properties
Feign会通过动态代理，帮我们生成实现类。
注解@FeignClient声明Feign的客户端，注解value指明服务名称
接口定义的方法，采用SpringMVC的注解。Feign会根据注解帮我们生成URL地址
注解@RequestMapping中的/user，不要忘记。因为Feign需要拼接可访问地址
```



(3)编写控制层

在`user-consumer`中创建`com.itheima.controller.ConsumerFeignController`，在Controller中使用@Autowired注入FeignClient,代码如下：

```java
@RestController
@RequestMapping(value = "/feign")
public class ConsumerFeignController {

    @Autowired
    private UserClient userClient;

    /****
     * 使用Feign调用user-provider的方法
     */
    @RequestMapping(value = "/{id}")
    public User queryById(@PathVariable(value = "id")Integer id){
        return userClient.findById(id);
    }
}
```



(4)开启Feign

修改`user-consumer`的启动类，在启动类上添加`@EnableFeignClients`注解，开启Feign,代码如下：

![1563261614474](images\1563261614474.png)

(5)测试

请求`<http://localhost:18082/feign/2>`，效果如下：

![1563261683709](images\1563261683709.png)



#### 1.2.3 负载均衡

Feign自身已经集成了Ribbon，因此使用Feign的时候，不需要额外引入依赖。

![1563261807063](images\1563261807063.png)

Feign内置的ribbon默认设置了请求超时时长，默认是10000，可以修改

ribbon内部有重试机制，一旦超时，会自动重新发起请求。如果不希望重试可以关闭配置：

```yml
# 修改服务地址轮询策略，默认是轮询，配置之后变随机
user-provider:
  ribbon:
    #轮询
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RoundRobinRule
    ConnectTimeout: 10000 # 连接超时时间
    ReadTimeout: 2000 # 数据读取超时时间
    MaxAutoRetries: 1 # 最大重试次数(第一个服务)
    MaxAutoRetriesNextServer: 0 # 最大重试下一个服务次数(集群的情况才会用到)
    OkToRetryOnAllOperations: false # 无论是请求超时或者socket read timeout都进行重试
```



#### 1.2.4 熔断器支持

feign整合Hystrix熔断器

Feign默认也有对Hystrix的集成!

![1563263001726](images\1563263001726.png)

实现步骤：

```properties
1. 在配置文件application.yml中开启feign熔断器支持
2. 编写FallBack处理类，实现FeignClient客户端
3. 在@FeignClient注解中，指定FallBack处理类。
4. 测试
```



(1)开启Hystrix

在配置文件application.yml中开启feign熔断器支持：默认关闭

```properties
feign:
  hystrix:
    enabled: true # 开启Feign的熔断功能
```

(2)熔断降级类创建

修改`user-consumer`,创建一个类`com.itheima.feign.fallback.UserClientFallback`，实现刚才编写的UserClient，作为FallBack的处理类,代码如下：

```java
@Component
public class UserClientFallback implements UserClient{

    /***
     * 服务降级处理方法
     * @param id
     * @return
     */
    @Override
    public User findById(Integer id) {
        User user = new User();
        user.setUsername("Fallback，服务降级。。。");
        return user;
    }
}
```



(3)指定Fallback处理类

在@FeignClient注解中，指定FallBack处理类

![1563263626561](images\1563263626561.png)



(4)测试

关闭服务消费方，请求`<http://localhost:18082/feign/2>`，效果如下：

![1563266845269](images\1563266845269.png)



#### 1.2.5 请求压缩

SpringCloudFeign支持对请求和响应进行GZIP压缩，以减少通信过程中的性能损耗。

通过配置开启请求与响应的压缩功能：

```yml
feign:
	compression:
        request:
            enabled: true # 开启请求压缩
        response:
            enabled: true # 开启响应压缩
```

也可以对请求的数据类型，以及触发压缩的大小下限进行设置

```yml
#  Feign配置
feign:
	compression:
		request:
			enabled: true # 开启请求压缩
			mime-types:	text/html,application/xml,application/json # 设置压缩的数据类型
			min-request-size: 2048 # 设置触发压缩的大小下限
			#以上数据类型，压缩大小下限均为默认值
```



#### 1.2.6 Feign的日志级别配置

通过loggin.level.xx=debug来设置日志级别。然而这个对Feign客户端不会产生效果。因为@FeignClient注解修饰的客户端在被代理时，都会创建一个新的Feign.Logger实例。我们需要额外通过配置类的方式指定这个日志的级别才可以。

**实现步骤：**

```properties
1. 在application.yml配置文件中开启日志级别配置
2. 编写配置类，定义日志级别bean。
3. 在接口的@FeignClient中指定配置类
4. 重启项目，测试访问
```



**实现过程：**

(1)普通日志等级配置

在`user-consumer`的配置文件中设置com.itheima包下的日志级别都为debug

```properties
# com.itheima 包下的日志级别都为Debug
logging:
  level: 
    com.itheima: debug
```

(2)Feign日志等级配置

在`user-consumer`中创建`com.itheima.feign.util.FeignConfig`,定义日志级别

```java
@Configuration
public class FeignConfig {

    /***
     * 日志级别
     * @return
     */
    @Bean
    public Logger.Level feignLoggerLevel(){
        return Logger.Level.FULL;
    }
}
```

日志级别说明：

```properties
Feign支持4中级别：
	NONE：不记录任何日志，默认值
	BASIC：仅记录请求的方法，URL以及响应状态码和执行时间
	HEADERS：在BASIC基础上，额外记录了请求和响应的头信息
	FULL：记录所有请求和响应的明细，包括头信息、请求体、元数据
```



(3)指定配置类

修改`user-consumer`的`com.itheima.feign.UserClient`指定上面的配置类，代码如下：

![1563267991954](images\1563267991954.png)



重启项目，即可看到每次访问的日志

![1563268061326](images\1563268061326.png)



### 1.3 小结

- Feign的作用:不再使用拼接URL的方式实现远程调用，以接口调用的方式实现远程调用，简化了远程调用的实现方式，增强了远程调用的功能，例如：增加了负载均衡、熔断、压缩、日志启用。

- 掌握Feign的使用过程

  ```properties
  1.引入Feign依赖包
  2.使用@EnabledFeignClients启用Feign功能
  3.创建Feign接口,feign接口中需要指定调用的服务名字
  ```

- 掌握Feign的负载均衡配置

  ```properties
  在配置文件中配置
  {spring.application.name}:ribbon:负载均衡属性配置
  ```

- 掌握Feign的熔断配置

  ```properties
  1.在application.yml中开启Hystrix
  2.给Feign接口创建一个实现类
  3.给Feign指定fallback类
  ```

- 掌握Feign的压缩配置

  ```properties
  在application.yml中指定压缩属性即可
  ```

- 掌握Feign的日志配置

  ```properties
  1.在application.yml中开启普通日志等级
  2.创建一个类，定义Feign日志等级
  3.在Feign接口中指定定义日志的配置
  ```

  

## 2 网关 Spring Cloud Gateway

![1563313032299](images\1563313032299.png)

### 2.1 目标

- 网关的作用
- 会配置动态路由 静态路由/动态路由
- 会配置过滤器 
  - 局部过滤器
  - ==全局过滤器==
- ==能自定义全局过滤器==
  - ==已经帮我们实现了很多种类的过滤器==
  - 自定的话:要学会如何自定义一个全局过滤器/局部过滤器(不要求掌握)



==网关作用：为微服务提供统一入口，然后对请求进行路由管理，可以在路由管理基础上进行一系列的过滤，可以做一系列的监控操作，限流。==



### 2.2 讲解

#### 2.2.1 Gateway 简介

Spring Cloud Gateway 是Spring Cloud团队的一个全新项目，基于Spring 5.0、SpringBoot2.0、Project Reactor 等技术开发的网关。 ==旨在为微服务架构提供一种简单有效统一的API路由管理方式。==

Spring Cloud Gateway 作为SpringCloud生态系统中的网关，目标是替代Netflix Zuul。Gateway不仅提供统一路由方式，并且==基于Filter链的方式提供网关的基本功能。例如：安全，监控/指标，和限流。==

**本身也是一个微服务，需要注册到Eureka** client

**网关的核心功能：**路由 过滤

localhost:8001/user/findAll

**核心概念：**通过画图解释

- **路由(route)：** 转发到哪里去:localhost:18081
- **断言Predicate函数：**/user
- **过滤器(Filter)：**

localhost:18081/user/findAll

#### 2.2.2 快速入门

**实现步骤：**

```properties
1. 创建gateway-service工程SpringBoot
2. 编写基础配置
3. 编写路由规则，配置静态路由策略
4. 启动网关服务进行测试
```





**实现过程：**

(1)创建一个子工程gateway-service

工程坐标：

```xml
<artifactId>gateway-service</artifactId>
<groupId>com.itheima</groupId>
<version>1.0-SNAPSHOT</version>
```



(2)pom.xml依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>springcloud-parent</artifactId>
        <groupId>com.itheima</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <artifactId>gateway-service</artifactId>

    <dependencies>
        <!--网关依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>
        <!-- Eureka客户端 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
    </dependencies>
</project>
```



(3)启动类

创建启动类`com.itheima.GatewayApplication`,代码如下：

```java
@SpringBootApplication
@EnableDiscoveryClient// 开启Eureka客户端发现功能
public class GatewayApplication {

    public static void main(String[] args) {
        SpringApplication.run(GatewayApplication.class,args);
    }
}
```



(4)application.yml配置

```properties
# 注释版本
server:
  port: 18084
spring:
  application:
    name: api-gateway # 应用名
# Eureka服务中心配置
eureka:
  client:
    service-url:
      # 注册Eureka Server集群
      defaultZone: http://127.0.0.1:7001/eureka
```



#### 2.2.3 路由配置

![1563269396043](images\1563269396043.png)

通过网关配置一个路由功能，用户访问网关的时候,如果用户请求的路径是以`/user`开始，则路由到`user-provider`服务去,修改application.yml配置即可实现，配置如下：

```properties
spring:
  application:
    # 应用名
    name: api-gateway
  cloud:
    gateway:
      routes:
        #id唯一标识，可自定义
        - id: user-service-route
          #路由的服务地址
          uri: http://localhost:18081
          # 路由拦截的地址配置（断言）
          predicates:
            - Path=/user/**
```



启动GatewayApplication测试

访问`http://localhost:18084/user/find/2`会访问`user-provider`服务，效果如下：

![1563269953975](images\1563269953975.png)





#### 2.2.4 动态路由

![1563270055671](images\1563270055671.png)

刚才路由规则中，我们把路径对应服务地址写死了！如果服务提供者集群的话，这样做不合理。应该是**根据服务名称**，去Eureka注册中心查找服务对应的所有实例列表，然后进行动态路由！

**修改映射配置：通过服务名称获取：**

修改application.yml

因为已经配置了Eureka客户端，可以从Eureka获取服务的地址信息，修改application.yml文件如下:

![1563270306529](images\1563270306529.png)

上图代码如下：

```properties
spring:
  application:
    # 应用名
    name: api-gateway
  cloud:
    gateway:
      routes:
        #id唯一标识，可自定义
        - id: user-service-route
          #路由的服务地址
          #uri: http://localhost:18081
          #lb协议表示从Eureka注册中心获取服务请求地址
          #user-provider访问的服务名称。
          #路由地址如果通过lb协议加服务名称时，会自动使用负载均衡访问对应服务
          uri: lb://user-provider
          # 路由拦截的地址配置（断言）
          predicates:
            - Path=/user/**
```

路由配置中uri所用的协议为lb时，gateway将把user-provider解析为实际的主机和端口，并通过Ribbon进行负载均衡。





#### 2.2.5 过滤器

过滤器作为Gateway的重要功能。常用于请求鉴权、服务调用时长统计、修改请求或响应header、限流、去除路径等等...

##### 2.2.5.1 过滤器的分类

```properties
默认过滤器：出厂自带，实现好了拿来就用，不需要实现
  全局默认过滤器
  局部默认过滤器
自定义过滤器：根据需求自己实现，实现后需配置，然后才能用哦。
  全局过滤器：作用在所有路由上。
  局部过滤器：配置在具体路由下，只作用在当前路由上。
```

默认过滤器几十个，常见如下：

| 过滤器名称           | 说明                         |
| -------------------- | ---------------------------- |
| AddRequestHeader     | 对匹配上的请求加上Header     |
| AddRequestParameters | 对匹配上的请求路由增加参数   |
| AddResponseHeader    | 对从网关返回的响应添加Header |
| StripPrefix          | 对匹配上的请求路径去除前缀   |

详细说明官方[链接](https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.1.1.RELEASE/single/spring-cloud-gateway.html#_gatewayfilter_factories)



##### 2.2.5.2 默认过滤器配置

默认过滤器有两个：全局默认过滤器和局部默认过滤器。

**全局过滤器：对输出响应头设置属性**

对输出的响应设置其头部属性名称为X-Response-Default-MyName,值为itheima

修改配置文件

```yml
spring:
  cloud:
    gateway:
     # 配置全局默认过滤器
      default-filters:
      # 往响应过滤器中加入信息
        - StripPrefix=2
```



查看浏览器响应头信息!

![1563271202588](images\1563271202588.png)



**局部过滤器：**通过局部默认过滤器，修改请求路径。局部过滤器在这里介绍两种：添加路径前缀、去除路径前缀。

**第一：添加路径前缀：**

在gateway中可以通过配置路由的过滤器PrefixPath 实现映射路径中的前缀

配置请求地址添加路径前缀过滤器

![1563271983446](images\1563271983446.png)

上图配置如下：

```yml
spring:
  application:
    # 应用名
    name: api-gateway
  cloud:
    gateway:
      routes:
        #id唯一标识，可自定义
        - id: user-service-route
          #路由的服务地址
          #uri: http://localhost:18081
          #lb协议表示从Eureka注册中心获取服务请求地址
          #user-provider访问的服务名称。
          #路由地址如果通过lb协议加服务名称时，会自动使用负载均衡访问对应服务
          uri: lb://user-provider
          # 路由拦截的地址配置（断言）
          predicates:
            - Path=/**
          filters:
            # 请求地址添加路径前缀过滤器
            - PrefixPath=/user
        - id: xiaobai
          #路由的服务地址
          #uri: http://localhost:18081
          #lb协议表示从Eureka注册中心获取服务请求地址
          #user-provider访问的服务名称。
          #路由地址如果通过lb协议加服务名称时，会自动使用负载均衡访问对应服务
          uri: lb://user-provider
          # 路由拦截的地址配置（断言）
          predicates:
            - Path=/**
          filters:
            # 请求地址添加路径前缀过滤器
            - PrefixPath=/user
      default-filters:
        - AddResponseHeader=X-Response-Default-MyName,itheima
```

路由地址信息：

| 配置             | 访问地址                      | 路由地址                           |
| ---------------- | ----------------------------- | ---------------------------------- |
| PrefixPath=/user | http://localhost:18084/find/2 | http://localhost:18081/user/find/2 |



**第二：去除路径前缀：**

在gateway中通过配置路由过滤器StripPrefix，实现映射路径中地址的去除。通过StripPrefix=1来指定路由要去掉的前缀个数。如：路径/api/user/1将会被路由到/user/1。

配置去除路径前缀过滤器

![1563272403821](images\1563272403821.png)

上图配置如下：

```yml
spring:
  application:
    # 应用名
    name: api-gateway
  cloud:
    gateway:
      routes:
        #id唯一标识，可自定义
        - id: user-service-route
          #路由的服务地址
          #uri: http://localhost:18081
          #lb协议表示从Eureka注册中心获取服务请求地址
          #user-provider访问的服务名称。
          #路由地址如果通过lb协议加服务名称时，会自动使用负载均衡访问对应服务
          uri: lb://user-provider
          # 路由拦截的地址配置（断言）
          predicates:
            - Path=/**
          filters:
            # 请求地址添加路径前缀过滤器
            #- PrefixPath=/user
            # 去除路径前缀过滤器
            - StripPrefix=1
      default-filters:
        - AddResponseHeader=X-Response-Default-MyName,itheima
```

路由地址信息：

| 配置          | 访问地址                                 | 路由地址                           |
| ------------- | ---------------------------------------- | ---------------------------------- |
| StripPrefix=1 | http://localhost:18084/api/user/find/2   | http://localhost:18081/user/find/2 |
| StripPrefix=2 | http://localhost:18084/api/r/user/find/2 | http://localhost:18081/user/find/2 |



##### 2.2.5.3 自定义过滤器案例

自定义过滤器也有两个：==全局自定义过滤器==，和局部自定义过滤器。

自定义全局过滤器的案例，自定义局部过滤器的案例。

**自定义全局过滤器的案例：**模拟登陆校验。

基本逻辑：如果请求中有Token参数，则认为请求有效放行，如果没有则拦截提示授权无效

校验步骤:

1.用户请求发送到gateway

2.自定义全局过滤器拦截请求,验证请求的url里面有没有token参数

3.有token正常返回数据/没有token返回410错误



###### 2.2.5.3.1 全局过滤器自定义：

**实现步骤：**

```
1.在gateway-service工程编写全局过滤器类GlobalFilter,Ordered
2.编写业务逻辑代码
3.访问接口测试，加token和不加token。
```



**实现过程：**

在`gateway-service`中创建`com.itheima.filter.LoginGlobalFilter`全局过滤器类,代码如下：

```java
@Component
public class LoginGlobalFilter implements GlobalFilter, Ordered {

    /***
     * 过滤拦截
     * @param exchange
     * @param chain
     * @return
     */
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        //获取请求参数
        String token = exchange.getRequest().getQueryParams().getFirst("token");

        //如果token为空，则表示没有登录
        if(StringUtils.isEmpty(token)){
            //没登录,状态设置403
            exchange.getResponse().setStatusCode(HttpStatus.PAYLOAD_TOO_LARGE);
            //结束请求
            return exchange.getResponse().setComplete();
        }
		
        chain.doFilter(exchange);
        //放行
        return chain.filter(exchange);
    }

    /***
     * 定义过滤器执行顺序
     * 返回值越小，越靠前执行
     * @return
     */
    @Override
    public int getOrder() {
        return 0;
    }
}
```



测试：不携带token  `<http://localhost:18084/api/user/find/2>`效果如下：

![1563273974252](images\1563273974252.png)

测试：携带token `<http://localhost:18084/api/user/find/2?token=abc>` 此时可以正常访问。



###### 2.2.5.3.2 局部过滤器定义

自定义局部过滤器，该过滤器在控制台输出配置文件中指定名称的请求参数及参数的值

**实现步骤：**

```properties
1. 在gateway-service中编写MyParamGatewayFilterFactory类
2. 实现业务代码：循环请求参数中是否包含name，如果包含则输出参数值
3. 修改配置文件
4. 访问请求测试，带name参数
```



**实现过程：**

在gateway_service中编写MyParamGatewayFilterFactory类

```java
@Component
public class MyParamGatewayFilterFactory extends AbstractGatewayFilterFactory<MyParamGatewayFilterFactory.Config> {
    /**
     * 定义需要处理的参数
     */
    public static final String PARAM_NAME = "name";

    /****
     * 处理过程
     * @param config
     * @return
     */
    @Override
    public GatewayFilter apply(MyParamGatewayFilterFactory.Config config) {
        return new GatewayFilter() {
            @Override
            public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
                String name = exchange.getRequest().getQueryParams().getFirst("name");
                if(!StringUtils.isEmpty(name)){
                    System.out.println("名字参数："+name);
                }
                String username = exchange.getRequest().getQueryParams().getFirst("username");
                if(!StringUtils.isEmpty(username)){
                    System.out.println("名字参数："+username);
                }
                return chain.filter(exchange);
            }
        };
    }

    /***
     * 构造函数
     */
    public MyParamGatewayFilterFactory() {
        super(MyParamGatewayFilterFactory.Config.class);
    }

    /***
     * 处理字段的排序
     * @return
     */
    @Override
    public List<String> shortcutFieldOrder() {
        return Arrays.asList(PARAM_NAME);
    }

    /****
     * 需要处理的参数
     * name：和处理的参数名字保持一致
     */
    public static class Config {
        private String name;

        public String getName() {
            return name;
        }
        public void setName(String name) {
            this.name = name;
        }
    }
}
```



修改application.yml配置文件

![1563316539267](images\1563316539267.png)

测试访问，检查后台是否输出name和itcast；访问`<http://localhost:18084/api/user/find/2?namea=itheima&tomen=aaa>`会输出。



#### 2.2.6 微服务架构加入Gateway后

![SpringCloud微服务架构--加入Gateway后 (1)](images\SpringCloud微服务架构--加入Gateway后 (1).jpg)

- 不管是来自客户端的请求，还是服务内部调用。一切对服务的请求都可经过网关。
- 网关实现鉴权、动态路由等等操作。
- Gateway就是我们服务的统一入口



#### 2.3 小结

- 网关的作用

  ```properties
  1.为微服务架构提供一种简单有效统一的API路由管理方式 (入口)
  2.可以在网关中实现微服务鉴权、安全控制、请求监控、限流
  ```

- 会配置静态和动态路由

  ```properties
  使用lb配置，能根据服务名字动态请求。lb://serviceID
  ```

- 会配置过滤器

  ```properties
  默认过滤器：default-filters: 全局和局部过滤器要求能够配置
  ```

- 能自定义全局过滤器

  ```properties
  编写过滤器类，实现GlobalFilter和Ordered，在filter方法中实现过滤。
  ```

  



## 3 配置中心 Spring Cloud Config

### 3.1 目标

- 了解配置中心的作用
- 能配置Git仓库
- 能搭建配置中心
- 修改微服务，从配置中心获取修改的配置

### 3.2 讲解

#### 3.2.1 Config简介

分布式系统中，由于服务数量非常多，配置文件分散在不同微服务项目中，管理极其不方便。为了方便配置文件集中管理，需要分布式配置中心组件。在Spring Cloud中，提供了Spring Cloud Config，它支持配置文件放在配置服务的本地，也支持配置文件放在远程仓库Git(GitHub、码云)。配置中心本质上是一个微服务，同样需要注册到Eureka服务中心！



![1558282481764](images\1558282481764.png)

​																			【配置中心的架构图】

#### 3.2.2 Git配置管理

##### 3.2.2.1 远程Git仓库

- 知名的Git远程仓库有国外的GitHub和国内的码云(gitee)；
- GitHub主服务在外网，访问经常不稳定，如果希望服务稳定，可以使用码云；
- 码云访问地址：http://gitee.com

测试地址：

```
https://gitee.com/skllll/config.git
```



##### 3.2.2.2 创建远程仓库

首先使用码云上的git仓库需要先注册账户

账户注册完成，然后使用账户登录码云控制台并创建公开仓库

![1563278785689](images\1563278785689.png)



配置仓库 名称和路径

![1563278922857](images\1563278922857.png)



##### 3.2.2.3 创建配置文件

在新建的仓库中创建需要被统一配置管理的配置文件

![1563278978078](images\1563278978078.png)

文件命名有规则：

```properties
配置文件的命名方式：{application}-{profile}.yml或{application}-{profile}.properties
application为应用名称
profile用于区分开发环境dev，测试环境test，生产环境pro等
  开发环境 user-dev.yml
  测试环境 user-test.yml
  生产环境 user-pro.yml
```

创建一个user-provider-dev.yml文件

将user-provider工程里的配置文件application.yml内容复制进去。

![1563279131699](images\1563279131699.png)



创建完user-provider-dev.yml配置文件之后，gitee中的仓库如下： 

![1563279208501](images\1563279208501.png)



#### 3.2.3 搭建配置中心微服务

**实现步骤：**

```properties
1. 创建配置中心SpringBoot项目config_server
2. 配置坐标依赖
3. 启动类添加开启配置中心服务注解
4. 配置服务中心application.yml文件
5. 启动测试
```



**实现过程：**

(1)创建工程

工程坐标

```properties
<artifactId>config-server</artifactId>
<groupId>com.itheima</groupId>
<version>1.0-SNAPSHOT</version>
```



(2)pom.xml依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>springcloud-parent</artifactId>
        <groupId>com.itheima</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <artifactId>config-server</artifactId>

    <dependencies>
        <!-- Eureka客户端 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <!--配置中心-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-config-server</artifactId>
        </dependency>
    </dependencies>
</project>
```



(3)创建启动类

在`config-server`工程中创建启动类`com.itheima.ConfigServerApplication`,代码如下：

```java
@SpringBootApplication
@EnableDiscoveryClient//开启Eureka客户端发现功能
@EnableConfigServer//开启配置服务支持
public class ConfigServerApplication {

    public static void main(String[] args) {
        SpringApplication.run(ConfigServerApplication.class,args);
    }
}
```



(4)application.yml配置文件

```properties
# 注释版本
server:
  port: 18085 # 端口号
spring:
  application:
    name: config-server # 应用名
  cloud:
    config:
      server:
        git:
          # 配置gitee的仓库地址
          uri: https://gitee.com/skllll/config.git
# Eureka服务中心配置
eureka:
  client:
    service-url:
      # 注册Eureka Server集群
      defaultZone: http://127.0.0.1:7001/eureka
# com.itheima 包下的日志级别都为Debug
logging:
  level:
    com: debug
```

注意：上述spring.cloud.config.server.git.uri是在码云创建的仓库地址



(5)测试

启动`config-server`，访问`<http://localhost:18085/user-provider-dev.yml>`，效果如下：

![1563276201436](images\1563276201436.png)

可以查看到码云上的文件数据，并且可以在gitee上修改user-dev.yml，然后刷新上述测试地址也能及时更新数据





#### 3.2.4 服务去获取配置中心配置

**目标：改造user-provider工程，配置文件不再由微服务项目提供，而是从配置中心获取。**

**实现步骤：**

```properties
1. 添加配置中心客户端启动依赖
2. 修改服务提供者的配置文件
3. 启动服务
4. 测试效果
```



**实现过程：**

(1)添加依赖

```xml
<!--spring cloud 配置中心-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

(2)修改配置

删除user-provider工程的application.yml文件

创建user-provider工程bootstrap.yml配置文件，配置内容如下

```yml
# 注释版本
spring:
  cloud:
    config:
      name: user-provider # 与远程仓库中的配置文件的application保持一致，{application}-{profile}.yml
      profile: dev # 远程仓库中的配置文件的profile保持一致
      label: master # 远程仓库中的版本保持一致
      discovery:
        enabled: true # 使用配置中心
        service-id: config-server # 配置中心服务id
#向Eureka服务中心集群注册服务
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:7001/eureka
```

关于application.yml和bootstrap.yml文件的说明：

```
- bootstrap.yml文件是SpringBoot的默认配置文件，而且其加载时间相比于application.yml更早。
- bootstrap.yml和application.yml都是默认配置文件，但定位不同
  - bootstrap.yml可以理解成系统级别的一些参数配置，一般不会变动
  - application.yml用来定义应用级别的参数
- 搭配spring-cloud-config使用application.yml的配置可以动态替换。
- bootstrap.yml相当于项目启动的引导文件，内容相对固定
- application.yml文件是微服务的常规配置参数，变化比较频繁
```



启动测试：

启动服务中心、配置中心、用户中心user_service![1558275102243](images\1558275102243.png)

如果启动没报错，其实已经使用上配置中心内容了

可以在服务中心查看也可以检验user_service的服务![1558275137843](images\1558275137843.png)



#### 3.2.5 配置中心存在的问题

(1)修改码云配置文件

修改在码云上的user-provider-dev.yml文件，添加一个属性test.message,如下操作：

![1563276549136](images\1563276549136.png)



(2)读取配置文件数据

在`user-provider`工程中创建一个`com.itheima.controller.LoadConfigController`读取配置文件信息，代码如下：

```java
@RestController
@RequestMapping(value = "/config")
public class LoadConfigController {

    @Value("${test.message}")
    private String msg;

    /***
     * 响应配置文件中的数据
     * @return
     */
    @RequestMapping(value = "/load")
    public String load(){
        return msg;
    }
}
```

启动运行`user-provider`，访问`<http://localhost:18081/config/load>`

![1563276955927](images\1563276955927.png)

修改码云上的配置后，发现项目中的数据仍然没有变化,只有项目重启后才会变化。



### 3.3 小结

- 配置中心的作用:将各个微服务的配置文件集中到一起进行统一管理。

- 能搭建配置中心

  ```
  需要在application.yml配置文件中指定需要远程更新的仓库地址。
  ```

- 修改微服务，从配置中心获取修改的配置

  ```properties
  创建bootstrap.yml，并在bootstrap.yml中配置
  # 注释版本
  spring:
    cloud:
      config:
        name: user-provider # 与远程仓库中的配置文件的application保持一致，{application}-{profile}.yml
        profile: dev # 远程仓库中的配置文件的profile保持一致
        label: master # 远程仓库中的版本保持一致
        discovery:
          enabled: true # 使用配置中心
          service-id: config-server # 配置中心服务id
  #向Eureka服务中心集群注册服务
  eureka:
    client:
      service-url:
        defaultZone: http://127.0.0.1:7001/eureka
  ```

  



## 4 消息总线 Spring Cloud Bus 

SpringCloud Bus，解决上述问题，实现配置自动更新。

注意：SpringCloudBus基于RabbitMQ实现，默认使用本地的消息队列服务，所以需要提前安装并启动RabbitMQ。安装参考./04资料/安装Windows RabbitMQ.pdf



### 4.1 Bus简介

Bus是用轻量的消息代理将分布式的节点连接起来，可以用于**广播配置文件的更改**或者服务的监控管理。

Bus可以为微服务做监控，也可以实现应用程序之间互相通信。Bus可选的消息代理**RabbitMQ**和Kafka。

广播出去的配置文件服务会进行本地缓存。RocketMQ--java

### 4.2 整合案例

目标：消息总线整合入微服务系统，实现配置中心的配置自动更新。不需要重启微服务。

#### 4.2.1 改造配置中心

**改造步骤：**

1. 在config-server项目中加入Bus相关依赖
2. 修改application.yml，加入RabbitMQ的配置信息，和暴露触发消息总线地址

**实现过程**：

(1)引入依赖

修改`config-server`的pom.xml引入依赖：

```xml
<!--消息总线依赖-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-bus</artifactId>
</dependency>
<!--RabbitMQ依赖-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-stream-binder-rabbit</artifactId>
</dependency>
```



(2)修改application.yml配置文件

修改`config-server`的application.yml，如下配置的rabbit都是默认值，其实可以完全不配置,代码如下：

![1563277655450](images\1563277655450.png)

上图配置如下：

```properties
# 注释版本
server:
  port: 18085 # 端口号
spring:
  application:
    name: config-server # 应用名
  cloud:
    config:
      server:
        git:
          # 配置gitee的仓库地址
          uri: https://gitee.com/skllll/config.git
  # rabbitmq的配置信息；如下配置的rabbit都是默认值，其实可以完全不配置
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest
# 暴露触发消息总线的地址
management:
  endpoints:
    web:
      exposure:
        # 暴露触发消息总线的地址
        include: bus-refresh

# Eureka服务中心配置
eureka:
  client:
    service-url:
      # 注册Eureka Server集群
      defaultZone: http://127.0.0.1:7001/eureka
# com.itheima 包下的日志级别都为Debug
logging:
  level:
    com: debug
```





#### 4.2.2 改造用户服务

**改造步骤：**

```properties
1. 在用户微服务user_service项目中加入Bus相关依赖
2. 修改user_service项目的bootstrap.yml，加入RabbitMQ的配置信息
3. UserController类上加入@RefreshScope刷新配置注解
4. 测试
```



**实现过程：**

(1)引入依赖

修改`user-provider`引入如下依赖：

```xml
<!--消息总线依赖-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-bus</artifactId>
</dependency>
<!--RabbitMQ依赖-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-stream-binder-rabbit</artifactId>
</dependency>
<!--健康监控依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```



(2)添加bootstrap.yml文件

在`user-provider`的resources目录下添加bootstrap.yml，添加rabbitmq配置，代码如下：

```properties
# 注释版本
spring:
  cloud:
    config:
      name: user-provider # 与远程仓库中的配置文件的application保持一致，{application}-{profile}.yml
      profile: dev # 远程仓库中的配置文件的profile保持一致
      label: master # 远程仓库中的版本保持一致
      discovery:
        enabled: true # 使用配置中心
        service-id: config-server # 配置中心服务id
# rabbitmq的配置信息；如下配置的rabbit都是默认值，其实可以完全不配置
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest
#向Eureka服务中心集群注册服务
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:7001/eureka
```



(3)添加刷新配置

修改`user-provider`的`com.itheima.controller.LoadConfigController`，添加一个`@RefreshScope`注解刷新配置信息，代码如下：

![1563278214919](images\1563278214919.png)

@RefreshScope：用于启用刷新配置文件的信息。



### 4.3 测试

目标：当我们修改Git仓库的配置文件，用户微服务是否能够在不重启的情况下自动更新配置文件信息。

测试步骤：

(1)启动`eureka-server`

(2)启动`config-server`

(3)启动`user-provider`

(4)访问测试

访问`<http://localhost:18081/config/load>`,效果如下：

![1563278362302](images\1563278362302.png)

(5)修改码云配置

修改码云的配置，修改后并提交，修改如下：

![1563278440773](images\1563278440773.png)



(6)刷新配置

使用Postman以POST方式请求`http://localhost:18085/actuator/bus-refresh`

![1563278498743](images\1563278498743.png)

请求地址中actuator是固定的，bus-refresh对应的是配置中心的config-server中的application.yml文件的配置项include的内容



(7)刷新测试

访问`<http://localhost:18081/config/load>`,效果如下：

![1563278633611](images\1563278633611.png)



**消息总线实现消息分发过程：**

- 请求地址访问配置中心的消息总线
- 消息总线接收到请求
- 消息总线向消息队列发送消息
- user-service微服务会监听消息队列
- user-service微服务接到消息队列中消息后
- user-service微服务会重新从配置中心获取最新配置信息





### SpringCloud 总架构图

![1558286268166](images\1558286268166.png)



课后要求:

1.实现以下feign的远程调用  负载均衡  熔断器  选修:日志 压缩

2.搭建好网关的微服务:加前缀 减前缀  实现自定义的全局过滤器

3.搭建以下配置中心,从远程仓库能加载到配置文件



