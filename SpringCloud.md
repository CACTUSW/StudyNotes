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
	Spring Cloud ->生态	基于SpringBoot
	
	1.Spring Cloud Netflix	一站式解决方案
	API网关,zuul组件
	Feign --- HTTPClient --- Http通信方式,同步,阻塞
	服务注册发现: Eureka
	熔断机制: Hystrix
	
	2.Apache Dubbo Zookeeper	半自动,需要整合别人的
	API:没有,找第三方插件,或者自己实现
	DUbbo
	Zookeeper
	没有: 借助Hystrix
	
	DUbbo这个方案并不完善
	
	3.Spring Cloud Alibaba	新的一站式解决方案
	
新概念: 服务网格~ Server Mesh

万变不离其宗
	1.Api
	2.HTTP,RPC
	3.注册和发现
	4.熔断机制
	
本质 ->网络不可靠
```

##  Question

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

|              | Dubbo         | SpringCloud                  |
| ------------ | ------------- | ---------------------------- |
| 服务注册中心 | Zookeeper     | Spring Cloud Netflix Eureka  |
| 服务调用方式 | RPC           | REST API                     |
| 服务监控     | Dubbo-monitor | Spring Boot Admin            |
| 断路器       | 不完善        | Spring Cloud Netflix Hystrix |
| 服务网关     | 无            | Spring Cloud Netflix Zuul    |
| 分布式配置   | 无            | Spring Cloud Config          |
| 服务跟踪     | 无            | Spring Cloud Sleuth          |
| 消息总线     | 无            | Spring Cloud Bus             |
| 数据流       | 无            | SpringCloud Stream           |
| 批量任务     | 无            | Spring Cloud Task            |

**最大区别:SpringCloud抛弃了Dubbo的RPC通信,采用的是基于HTTP的REST方式**

严格来说,这两种方式各有优劣,虽然从一定程度上来说,后者牺牲了服务调用的性能,但也避免了上面提到的原生RPC带来的问题.而且REST相比RPC更为灵活,服务提供方和调用方的依赖只依靠一纸契约,不存在代码级别的前依赖,这在强调快速演化的微服务环境下,显的更加合适
