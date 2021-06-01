# Swagger

学习目标：

+ 了解Swagger的作用和概念
+ 了解前后端分离
+ 在SpringBoot中继承Swagger

## Swagger简介

`前后端分离：`

+ 后端：后端控制层、服务层、数据访问层
+ 前端：前端控制层、视图层
  + 伪造后端数据，json。不需要后端，前端依旧可以运行
+ 前后端交互==>API
+ 前后端相对独立，松耦合
+ 前后端甚至可以部署在不同的服务器上

`产生的问题：`

+ 前后端集成联调，前后端人员无法协商，最终。。。

`解决方法：`

+ 首先指定schema[计划的提纲]，实时更新API，降低集成风险;
+ 早些年：指定Word计划文档；
+ 前后端分离：
  + 前端测试后端接口：postman
  + 后端提供接口，需要实时更新最新的消息及改动

### Swagger

+ 号称最流行的API框架
+ RestFul API文档在线生成工具=>API文档与API定义同步更新
+ 直接运行，可以在线测试API接口
+ 支持多种语言：（java，PHP...）

官网:https://swagger.io/

在项目中使用Swagger需要springbox

+ Swagger2
+ UI

## SpringBoot集成Swagger

导入依赖：

+ Springfox Swagger UI

  ```xml
  <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
  <dependency>
      <groupId>io.springfox</groupId>
      <artifactId>springfox-swagger-ui</artifactId>
      <version>3.0.0</version>
  </dependency>
  ```

+ Springfox Swagger2

  ```xml
  <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
  <dependency>
      <groupId>io.springfox</groupId>
      <artifactId>springfox-swagger2</artifactId>
      <version>3.0.0</version>
  </dependency>
  ```

  