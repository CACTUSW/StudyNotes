# SpringCloud

**阶段学习**

```markdown
三层架构 + MVC

框架：
    Spring IOC AOP

    SpringBoot，新一代的JavaEE开发标准 ——>自动装配

    模块化&all in one ->代码不变

微服务架构4个核心问题
    1.服务很多,客户端该怎么访问?
    2.这么多服务,服务间该如何通信?
    3.这么多服务该如何治理?
    4.服务挂了怎么办?    

解决方案:
    Spring Cloud ->生态    基于SpringBoot

    1.Spring Cloud Netflix    一站式解决方案
    API网关,zuul组件
    Feign --- HTTPClient --- Http通信方式,同步,阻塞
    服务注册发现: Eureka
    熔断机制: Hystrix

    2.Apache Dubbo Zookeeper    半自动,需要整合别人的
    API:没有,找第三方插件,或者自己实现
    DUbbo
    Zookeeper
    没有: 借助Hystrix

    DUbbo这个方案并不完善

    3.Spring Cloud Alibaba    新的一站式解决方案

新概念: 服务网格~ Server Mesh

万变不离其宗
    1.Api
    2.HTTP,RPC
    3.注册和发现
    4.熔断机制

本质 ->网络不可靠
```

## Question

1. 什么是微服务?
2. 微服务之间如何独立通信?
3. SpringCloud和DUbbo有哪些区别?
4. 谈谈对SpringBoot和SpringCloud的理解
5. 什么是服务熔断?什么是服务降级?
6. 微服务的优缺点分别是什么
7. 常见的微服务技术栈有哪些?
8. eureka和Zookeeper都可以提供服务注册和发现的功能,两者的区别?

## SpringCloud概述

### 什么是SpringCloud?

### SpringCloud和SpringBoot关系

+ SpringBoot专注于快速方便的开发单个个体微服务。 -jar
+ SpringCloud是关注全局的微服务协调整理治理框架,它将SpringBoot开发的一个个单体微服务整合并管理起来,为各个微服务之间提供服务:配置管理，服务发现，断路器，路由，微代理，事件总线，全局锁，决策竞选，分布式会话等等集成服务。
+ SpringBoot可以离开SpringCloud单独使用，开发项目，但是SpringCloud离不开SpringBoot，属于依赖关系
+ SpringBoot专注于快速、方便的开发单个个体微服务，SpringCloud关注全局的服务治理框架

### Dubbo和SpringCloud技术选型

#### 分布式+服务治理Dubbo

目前成熟的互联网框架：应用服务化拆分+消息中间件

#### Dubbo和SpringCloud对比

|        | Dubbo         | SpringCloud                  |
| ------ | ------------- | ---------------------------- |
| 服务注册中心 | Zookeeper     | Spring Cloud Netflix Eureka  |
| 服务调用方式 | RPC           | REST API                     |
| 服务监控   | Dubbo-monitor | Spring Boot Admin            |
| 断路器    | 不完善           | Spring Cloud Netflix Hystrix |
| 服务网关   | 无             | Spring Cloud Netflix Zuul    |
| 分布式配置  | 无             | Spring Cloud Config          |
| 服务跟踪   | 无             | Spring Cloud Sleuth          |
| 消息总线   | 无             | Spring Cloud Bus             |
| 数据流    | 无             | SpringCloud Stream           |
| 批量任务   | 无             | Spring Cloud Task            |

**最大区别:SpringCloud抛弃了Dubbo的RPC通信,采用的是基于HTTP的REST方式**

严格来说,这两种方式各有优劣,虽然从一定程度上来说,后者牺牲了服务调用的性能,但也避免了上面提到的原生RPC带来的问题.而且REST相比RPC更为灵活,服务提供方和调用方的依赖只依靠一纸契约,不存在代码级别的前依赖,这在强调快速演化的微服务环境下,显的更加合适。

## REST

### 总体介绍

+ 使用一个Dept部门模块做一个微服务通用案例Consumer（Client）通过REST调用Provider提供者（Server）提供的服务。

+ 回忆Spring，SpringMVC，MyBatis等以往学过的知识...

+ Maven的分包分模块架构复习
  
  ```markdown
  一个简单的Maven模块结构是这样的:
  
  -- app-parent: 一个父类项目(app-parent)聚合了很多子项目(app-util,app-dao,app-web...)
      |-- pom.xml
      |
      |-- app-core
      ||---- pom.xml
      |
      |-- app-web
      ||---- pom.xml
      ......
  ```
  
  一个父工程带着多个子Module子模块
  MicroServiceCloud父工程(Project)下初次带着3个子模块(Module)
  
  + microservicecloud-api[封装的整体entity/接口/公共配置等]
  + microservicecloud-provider-dept-8001[服务提供者]
  + microservicecloud-consumer-dept-80[服务消费者]

### SpringCloud版本选择

| SpringBoot | SpringCloud  | 关系                                    |
| ---------- | ------------ | ------------------------------------- |
| 1.2.x      | Angel版本      | 兼容SpringBoot 1.2.x                    |
| 1.3.x      | Brixton版本    | 兼容SpringBoot1.3.x,也兼容SpringBoot 1.4.x |
| 1.4.x      | Camden版本     | 兼容SpringBoot1.4.x,也兼容SpringBoot 1.5.x |
| 1.5.x      | Dalston版本    | 兼容SpringBoot1.4.x,不兼容SpringBoot 1.5.x |
| 1.5.x      | Edgware版本    | 兼容SpringBoot1.5.x,不兼容SpringBoot 2.0.x |
| 2.0.x      | Finchley版本   | 兼容SpringBoot1.4.x,不兼容SpringBoot 2.0.x |
| 2.1.x      | Greenwitch版本 |                                       |
|            |              |                                       |

## Eureka服务注册与发现

### 什么是Eureka

+ Eureka：发音？
+ Netflix在设计Eureka时，遵循的就是AP原则
+ Eureka是Netflix的一个子模块，也是核心模块之一。Eureka是一个基于REST的服务，用于定位服务，以实现云端中间层服务发现和故障转移，服务注册与发现对于微服务来说非常重要，有了服务发现与注册，只需要使用服务的标识符，就可以访问到服务，而需要修改服务调用的配置文件了，功能类似于Dubbo的注册中心，比如Zookeeper。

### 原理

+ Eureka的基本架构
  
  + Spring Cloud封装了Netflix公司开发的Eureka模块来实现服务注册和发现（对比Zookeeper）
  + Eureka采用了C/S的架构设计，Eureka作为服务注册功能的服务器，是服务注册中心
  + 而系统中的其他微服务，使用Eureka的客户端连接到EurekaServer。这样系统的维护人员就可以通过EurekaServer来监控系统中的各个微服务是否正常运行。SpringCloud的一些其他模块（比如Zuul）就可以通过Eureka Server来发现系统中的其他微服务，并执行相关的逻辑
  + Eureka包含两个组件：EurekaServer 和 Eureka Client
  + Eureka Server提供服务注册服务，各个节点启动后，会在Eureka Server中进行注册，这样Eureka Server中的服务注册表中将会列出所有可用的服务节点的信息，服务节点的信息可以在界面中直观的看到。
  + Eureka Client是一个Java客户端，用于简化Eureka Server的交互，客户端同时也具备一个内置的，使用轮询负载算法的负载均衡器。在应用启动后，将会向Eureka Server发送心跳（默认周期30秒）如果Eureka Server在多个心跳周期没有接收到某个节点的心跳，Eureka Server将会从服务注册表中把这个服务节点移除掉（默认周期为90秒）

+ 三大角色
  
  + Eureka Server：提供服务的注册与发现
  + Service Provider：将自身服务注册到Eureka中，从而使消费方能够找到
  + Service Consumer：服务消费方从Eureka中回去注册服务列表，从而找到消费服务。

### 构建

#### Eureka Server

pom.xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>SpringCloud</artifactId>
        <groupId>org.example</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>springcloud-eureka-7001</artifactId>

    <properties>
        <maven.compiler.source>15</maven.compiler.source>
        <maven.compiler.target>15</maven.compiler.target>
    </properties>

    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-eureka-server -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
            <version>3.0.3</version>
        </dependency>

    </dependencies>

</project>
```

yml文件配置

```yaml
server:
  port: 7001
#Eureka配置
eureka:
  instance:
    hostname: localhost #Eureka服务端的实例名字
  client:
    register-with-eureka: false #表示是否向Eureka注册中心注册自己
    fetch-registry: false #如果为false，则表示自己为注册中心
    service-url: # 监控页面
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

主启动类

```java
package com.volerde.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

/**
 * The type Eureka server 7001.
 *
 * @Author Volerde
 * @Date 2021 /6/14 15:27
 */
@SpringBootApplication
@EnableEurekaServer
public class EurekaServer7001 {
    /**
     * The entry point of application.
     *
     * @param args the input arguments
     */
    public static void main(String[] args) {
        SpringApplication.run(EurekaServer7001.class,args);
    }
}
```

测试结果

<img src="http://lsky.volerde.space/i/2022/05/16/62824514703b5.png" alt="1652704529984.png" style="zoom:80%;" />

#### Service Provider

pom配置（部分）

```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
<!--        完善监控信息-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
```

yml文件配置(部分)

```yaml
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka/
  instance:
    instance-id: springcloud-provider-dept8001
```

主启动类

```java
package com.volerde.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

/**
 * The type Dept provider 8001.
 *
 * @Author Volerde
 * @Date 2021 /6/11 15:59 启动类
 */
@SpringBootApplication
@EnableEurekaClient //加入Eureka的注解
public class DeptProvider8001 {
    /**
     * The entry point of application.
     *
     * @param args the input arguments
     */
    public static void main(String[] args) {
        SpringApplication.run(DeptProvider8001.class,args);
    }
}
```

### Eureka的自我保护机制

某时刻某一个微服务不可用了，Eureka不会立刻清理，依旧会对该服务的信息进行保存

+ 默认情况下，如果Eureka Server在一定时间内没有接受到某个微服务实例的心跳，Eureka Server将会注销该实例（默认90秒）。当网络分区发生故障时，微服务与Eureka之间无法正常通信，以上行为就可能会变得非常危险--因为微服务本身是健康的，此时本不应该注销这个服务。Eureka通过自我保护机制来解决这个问题--当Eureka Server节点在短时间内丢失过多客户端时（可能发生了网络分区故障），那么这个节点会进入自我保护模式。一旦进入该模式，Eureka Server就会保护服务注册表中的信息，不再删除服务注册表中的数据（不会注销任何微服务）当网络故障恢复后，该Eureka Server节点就会退出自我保护模式
+ 在自我保护模式中，EurekaServer会保护服务注册表中的信息,不再注销任何服务实例。当它收到的心跳数重新恢复到阈值以上时，该Eureka Server 节点就会自动退出自我保护模式。它的设计哲学就是宁可保留错误的服务注册信息，也不盲目注销任何可能健康的服务实例。一句话:好死不如赖活着
+ 综上，自我保护模式是一种应对网络异常的安全保护措施。它的架构哲学是宁可同时保留所有微服务(健康的微服务和不健康的微服务都会保留)，也不盲目注销任何健康的微服务。使用自我保护模式，可以让Eureka集群更加的健壮和稳定
+ 在SpringCloud中，可以使用**eureka. server. enable-self-preservation = false**禁用自我保护模式
  [不推荐关闭自我保护机制]

### 8001服务发现Discovery

+ 对于注册进Eureka里面的微服务，可以通过服务发现来获得该服务的信息。【对外暴露服务】

```java
/**
 * 获取注册进来的微服务的消息(用于多人协作)
 *
 * @return the object
 */
@GetMapping("/dept/discovery")
public Object discovery(){
    //获取微服务列表的清单
    List<String> services = client.getServices();
    System.out.println(services);

    //得到一个微服务的具体信息
    List<ServiceInstance> instances = client.getInstances("SPRINGCLOUD-PROVIDER-DEPT");
    for (ServiceInstance instance : instances) {
        System.out.println(
                instance.getHost()+"\t"+
                instance.getPort()+"\t"+
                instance.getUri()+"\t"+
                instance.getServiceId()
        );
    }
    return this.client;
}
```

### 集群环境配置

yml配置

```yaml
server:
  port: 7001
#Eureka配置
eureka:
  instance:
    hostname: localhost #Eureka服务端的实例名字
  client:
    register-with-eureka: false #表示是否向Eureka注册中心注册自己
    fetch-registry: false #如果为false，则表示自己为注册中心
    service-url: # 监控页面
#   (单机)   defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
#   (集群)   defaultZone: http://localhost:7002/eureka/,http://localhost:7003/eureka/
      defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
```

```yaml
server:
  port: 7002
#Eureka配置
eureka:
  instance:
    hostname: localhost #Eureka服务端的实例名字
  client:
    register-with-eureka: false #表示是否向Eureka注册中心注册自己
    fetch-registry: false #如果为false，则表示自己为注册中心
    service-url: # 监控页面
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7003.com:7003/eureka/
```

```yaml
server:
  port: 7003
#Eureka配置
eureka:
  instance:
    hostname: localhost #Eureka服务端的实例名字
  client:
    register-with-eureka: false #表示是否向Eureka注册中心注册自己
    fetch-registry: false #如果为false，则表示自己为注册中心
    service-url: # 监控页面
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/
```

8001服务配置

```yaml
server:
  port: 8001
mybatis:
  type-aliases-package: com.volerde.springcloud.pojo
  mapper-locations: classpath:mybatis/mapper/*.xml
  configuration:
    map-underscore-to-camel-case: true
spring:
  application:
    name: springcloud-provider-dept
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/springcloud01?Unicode=true&characterEncoding=utf-8&serverTimezone=UTC
    username: root
    password: 2312011989
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
  instance:
    instance-id: springcloud-provider-dept8001
```

### CAP原则及对比Zookeeper

#### 回顾CAP原则

RDBMS (Mysql. Oracle、 sqlServer) =\=\=>ACID
        NoSQL (redis、 mongdb) =\=\=> CAP

#### ACID是什么?

+ A (Atomicity) 原子性

+ C (Consistency)-致性

+ I(Isolation) 隔离性

+ D (Durability)持久性

#### CAP是什么?

+ C (Consistency)强一致性

+ A (Availbility) 可用性

+ P(Partition tolerance) 分区容错性

CAP的三进二: CA、AP、CP

**CAP理论的核心**

+ 一个分布式系统不可能同时很好的满足一 致性，可用性和分区容错性这三个需求
+ 根据CAP原理，将NoSQL数据库分成了满足CA原则，满足CP原则和满足AP原则三大类:
  + CA:单点集群,满足一致性,可用性的系统，通常可扩展性较差
  + CP:满足-致性，分区容错性的系统，通常性能不是特别高   
  + AP:满足可用性,分区容错性的系统，通常可能对一致性要求低一些

#### 作为服务注册中心，Eureka比kZookeeper好在哪里?

著名的CAP理论指出，-个分布式系统不可能同时满足C (- 致性)、A (可用性)、P (容错性)。

由于分区容错性P在分布式系统中是必须要保证的，因此我们只能在A和C之间进行权衡。

+ Zookeeper保证的是CP
+ Eureka保证的是AP

**Zookeeper保证的是CP**

当向注册中心查询服务列表时，我们可以容忍注册中心返回的是几分钟以前的注册信息，但不能接受服务直接down掉不可用。也就是说，服务注册功能对可用性的要求要高于一致性。 但是zk会出现这样一种情况， 当master节点因为网络故障与其他节点失去联系时,剩余节点会重新进行leader选举。问题在于，选举leader的时间太长,30~120s，且选举期间整个zk集群都是不可用的,这就导致在选举期间注册服务瘫痪。在云部署的环境下，因为网络问题使得zk集群失去master节点是较大概率会发生的事件，虽然服务最终能够恢复,但是漫长的选举时间导致的注册长期不可用是不能容忍的。

**Eureka保证的是AP**
Eureka看明白了这一-点, 因此在设计时就优先保证可用性。TEureka各个节点都是平等的，几个节点挂掉不会影响正常节点的工作,剩余的节点依然可以提供注册和查询服务。而Eureka的客户端在向某 个Eureka注册时，如果发现连接失败，则会自动切换至其他节点，只要有一台Eureka还在，就能保住注册服务的可用性,只不过查到的信息可能不是最新的，除此之外, Eureka还有一 种自我保护机制，如果在15分钟内超过85%的节点都没有正常的心跳，那么Eureka就认为客户端与注册中心出现了网络故障,此时会出现以下几种情况:

1. Eureka不再从注册列表中移除因为长时间没收到心跳而应该过期的服务
2. Eureka仍然能够接受新服务的注册和查询请求，但是不会被同步到其他节点上(即保证当前节点依然可用)
3. 当网络稳定时，当前实例新的注册信息会被同步到其他节点中

==因此，Eureka可以很好的应对因网络故障导致部分节点失去联系的情况，而不会像zookeeper那样使整个注册服务瘫痪==

## Ribbon

### ribbon是什么?

+ Spring Cloud Ribbon是基于Netflix Ribbon实现的一套==客户端负载均衡的工具==。

+ 简单的说，Ribbon是Netflix发布的开源项目,主要功能是提供客户端的软件负载均衡算法，将NetFlix的中间层服务连接在一起。Ribbon的客户端组件提供一系列完整的配置项如:连接超时、重试等等。简单的说，就是在配置文件中列出LoadBalancer[^1] 后面所有的机器，Ribbon会 自动的帮助你基于某种规则(如简单轮询，随机连接等等)去连接这些机器。我们也很容易使用Ribbon实现自定义的负载均衡算法!

### ribbon能干嘛?

+ LB,即负载均衡(Load Balance) ，在微服务或分布式集群中经常用的一种应用。
+ 负载均衡简单的说就是将用户的请求平摊的分配到多个服务上,从而达到系统的HA (高可用)。
+ 常见的负载均衡软件有Nginx[^2], LVs 等等
+ dubbo、 SpringCloud中均给我们提供了负载均衡, SpringCloud的负载均衡算法可以自定义
+ 负载均衡简单分类:
  + 集中式LB
    + 即在服务的消费方和提供方之间使用独立的LB设施，如Nginx, 由该设施负责把访问请求通过某种策
      略转发至服务的提供方!
  + 进程式LB
    + 将LB逻辑集成到消费方,消费方从服务注册中心获知有哪些地址可用,然后自己再从这些地址中选出一个合适的服务器。
    + Ribbon就属于进程内LB，它只是一个类库，集成于消费方进程，消费方通过它来获取到服务提供方的地址

80消费端配置

pom配置

```xml
<dependency><!--Ribbon依赖导入时出现问题，故用此依赖代替-->
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-eureka -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka</artifactId>
    <version>1.4.7.RELEASE</version>
</dependency>
```

yaml配置

```yaml
server:
  port: 80
#Eureka配置
eureka:
  client:
    register-with-eureka: false
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
```

主启动类配置

```java
package com.volerde.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;


/**
 * The type Dept consumer 80.
 *Ribbon&Eureka整合以后，客户端可以直接调用，不用关心IP地址和端口号
 * @Author Volerde
 * @Date 2021 /6/11 18:17
 */
@SpringBootApplication
@EnableEurekaClient
public class DeptConsumer80 {
    /**
     * The entry point of application.
     *
     * @param args the input arguments
     */
    public static void main(String[] args) {
        SpringApplication.run(DeptConsumer80.class,args);
    }
}
```

## Feign负载均衡(了解)

### 简介

feign是声明式的web service客户端，它让微服务之间的调用变得更简单了，类似controller调用service。 SpringCloud集成了Ribbon和Eureka,可在使用Feign时提供负载均衡的http客户端。只需要创建一个接口， 然后添加注解即可!

feign,主要是社区,大家都习惯面向接口编程。这个是很多开发人员的规范。调用微服务访问两种方法

1. 微服务名字[ ribbon]
2. 接口和注解[feign ]

**Feign能干什么?**

+ Feign旨在使编写Java Http客户端变得更容易
+ 前面在使用Ribbon + RestTemplate时,利用RestTemplate对Http请求的封装处理，形成了一套模板化的调用方法。但是在实际开发中，由于对服务依赖的调用可能不止一处，往往-个接口会被多处调用, 所以通常都会针对每个微服务自行封装一些客户端类来包装这些依赖服务的调用。 所以，Feign在此基础 上做了进一步封装,由他来帮助我们定义和实现依赖服务接口的定义，==在Feign的实现下，我们只需要创建一个接口并使用注解的方式来配置它[^3]==即可完成对服务提供方的接口绑定，简化了使用Spring　Cloud Ribbon时，自动封装服务调用客户端的开发量。

**Feign集成了Ribbon**

+ 利用Ribbon维护了MicroServerCloud-Dept的服务列表信息，并且通过轮询实现了客户端的负载均衡，而与Ribbon不同的是，通过Feign只需要定义服务绑定接口且以声明式的方法，简单的实现了服务调用。

### Feign使用步骤

api-module中配置server层

```java
package com.volerde.springcloud.service;

import com.volerde.springcloud.pojo.Dept;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;

import java.util.Date;
import java.util.List;

/**
 * The interface Dept client service.
 *
 * @Author Volerde
 * @Date 2021 /6/16 9:46
 */

@FeignClient(value ="SPRINGCLOUD-PROVIDER-DEPT")
public interface DeptClientService {

    /**
     * Query by id dept.
     *
     * @param id the id
     * @return the dept
     */
    @GetMapping("/dept/get/{id}")
    public Dept queryById(@PathVariable("id") Long id);

    /**
     * Query all dept.
     *
     * @return the dept
     */
    @GetMapping("/dept/list")
    public List<Date> queryAll();

    /**
     * Add dept dept.
     *
     * @param dept the dept
     * @return the dept
     */
    @PostMapping("/dept/add")
    public Boolean addDept(Dept dept);

}
```

consumer-module端添加注解

```java
package com.volerde.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import org.springframework.cloud.openfeign.EnableFeignClients;
import org.springframework.context.annotation.ComponentScan;


/**
 * The type Dept consumer 80.
 *Ribbon&Eureka整合以后，客户端可以直接调用，不用关心IP地址和端口号
 * @Author Volerde
 * @Date 2021 /6/11 18:17
 */
@SpringBootApplication
@EnableEurekaClient
@EnableFeignClients(basePackages = {"com.volerde.springcloud"})
public class FeignDeptConsumer80 {
    /**
     * The entry point of application.
     *
     * @param args the input arguments
     */
    public static void main(String[] args) {
        SpringApplication.run(FeignDeptConsumer80.class,args);
    }
}
```

## Hystrix

**分布式系统面临的问题**
复杂分布式体系结构中的应用程序有数-十个依赖关系,每个依赖关系在某些时候将不可避免的失败!

**服务雪崩**

多个微服务之间调用的时候，假设微服务A调用微服务B和微服务C,微服务B和微服务C又调用其他的微服务,这就是所谓的"扇出”、如果扇出的链路上某个微服务的调用响应时间过长或者不可用，对微服务A的调用就会占用越来越多的系统资源，进而引起系统崩溃，所谓的“雪崩效应"。

对于高流量的应用来说，单- -的后端依赖可能会导致所有服务器上的所有资源都在几秒中内饱和。比失败更糟糕的是，这些应用程序还可能导致服务之间的延迟增加，备份队列，线程和其他系统资源紧张，导致整个系统发生更多的级联故障，这些都表示需要对故障和延迟进行隔离和管理，以便单个依赖关系的失败，不能取消整个应用程序或系统。

我们需要==弃车保帅==

**什么是Hystrix**

Hystrix是一个用于处理分布式系统的延迟和容错的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时，异常等，Hystrix能够保证在一个依赖出问题的情况下，不会导致整体服务失败，避免级联故障，以提高分布式系统的弹性。

"断路器"本身是一种开关设置，当某个服务单元发生故障后，通过断路器的故障监控（类似熔断保险丝），==向调用方法返回一个服务预期的，可处理的预备响应（FallBack），而不是长时间的等待或者抛出调用方法无法处理的异常，这样就可以保证服务调用方的线程不会被长时间，不必要的占用==，从而避免了故障在分布式系统中的蔓延，乃至雪崩

**Hystrix能干什么**

+ 服务降级
+ 服务熔断
+ 服务限流
+ 接近事实的监控
+ ......

[官网Wiki](https://github.com/Netflix/Hystrix/wiki)

### 服务熔断

**是什么？**

熔断机制是应对雪崩效应的一种微服务链路保护机制。

当删除链路的某个微服务不可用或者响应时间太长时，会进行服务的降级，= =进而熔断该节点微服务的调用，快速返回错误的响应信息==。当检测到该节点微服务调用响应正常后恢复调用链路。在SpringCloud框架里熔断机制通过Hystrix实现。Hystrix会监控微服务间调用的状况，当失败的调用到一定阈值，缺省是5秒内20次调用失
败就会启动熔断机制。熔断机制的注解 是@HystrixCommand.

[^1]: 简称LB:负载均衡
反向代理服务器
类似于以前Dao接口上标注Mapper注解，现在是一个微服务接口 上面标注一个Feign注解即可。
