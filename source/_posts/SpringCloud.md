---
title: SpringCloud
date: 2023/4/10 11:21:25
tags:
  - SpringCloud
categories:
  - 分布式
cover: /img/springcloud/springcloud.jpg
---

# SpringCloud

## 回顾之前的知识

- JavaSE
- 数据库
- 前端
- Servlet
- Http
- Mybatis
- Spring
- SpringMVC
- SpringBoot
- Dubbo、Zookeeper、分布式基础
- Maven、git
- Ajax、json

## 这个阶段应该如何学

```java
三层架构+MVC

框架：
    Spring IOC AOP（解决企业开发的复杂性）
	  
    SpringBoot ： 自动装配
    
	SpringCloud
    
微服务4个核心问题：
    客户端如何访问（负载均衡）
    这么多服务？服务之间如何通信（Http、rpc）
    如何治理这么多服务（注册与发现）
    服务挂了怎么办
   
SpringCloud 生态！
    1. Spring Cloud NetFlix 一站式解决方案
    	api 网关 zuul组件
    	Feign --HttpClient ----Http通信方式，同步，阻塞
    	服务注册与发现：Eureka
    	熔断机制：Hystrix
    	2018年停止维护
    
    2. Apache Dubbo Zookeeper 半自动，需要整合别人的
    	API: 没有，找第三方组件，或者自己实现
        Dubbo：rpc通信框架（基于java的rpc）通信框架
    	Zookeeper
        没有：借助Hystrix
        
        Dubbo这个方案并不完善~
            
    3. Spring Cloud Alibaba 一站式解决方案
        
新概念：服务网格~ Server Mesh**
```

## 微服务优缺点

###### 优点

- 单一职责原则
- 每个服务足够内聚，足够小，代码容易理解
- 开发简单，开发效率
- 松耦合，在开发和部署阶段都是独立的
- 只是业务逻辑的代码

###### 缺点

- 开发人员要处理分布式系统的复杂性
- 多服务运维、运维成本
- 通信成本
- 数据一致性
- 集成测试
- 性能监控

###### NetFlix中文文档

https://www.springcloud.cc/spring-cloud-netflix.html

https://springcloud.cc/spring-cloud-dalston.html

## 服务提供者

###### 版本选择（Greenwich)--用f以上

## 服务消费者

缺少service采用RestTemplate来帮忙做请求

```

```

## Eureka

###### 定义

- 遵循AP原则
- 用于定位服务

###### 框架图

![image-20230326175511661](C:\Users\Megumi\AppData\Roaming\Typora\typora-user-images\image-20230326175511661.png)

- EurekaServer服务注册服务器

- 通过心跳EurekaServer来监控系统中各个微服务是否正常运行

  ```java
  package com.zane.springcloud;
  
  import org.springframework.boot.SpringApplication;
  import org.springframework.boot.autoconfigure.SpringBootApplication;
  import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;
  
  @SpringBootApplication
  @EnableEurekaServer // 服务端的启动类，可以接受别人注册进来
  public class EurekaServer_7001 {
      public static void main(String[] args) {
          SpringApplication.run(EurekaServer_7001.class,args);
      }
  }
  
  ```

  ```yml
  server:
    port: 7001
  
  eureka:
    instance:
      hostname: localhost #Eureka服务端实例名称
    client:
      register-with-eureka: false # 表示是否像eureka注册中心注册自己
      fetch-registry: false # 如果为false则表示自己为注册中心
      service-url: #监控页面
        defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
  ```

- 默认页面

  ![image-20230326181257728](C:\Users\Megumi\AppData\Roaming\Typora\typora-user-images\image-20230326181257728.png)

- 服务提供者

  ```xml
  <dependency>
              <groupId>org.springframework.cloud</groupId>
              <artifactId>spring-cloud-starter-eureka</artifactId>
              <version>1.4.7.RELEASE</version>
          </dependency>
  ```

  ```yml
  #Eureka的配置,服务注册到哪里
  eureka:
    client:
      service-url:
        defaultZone: http://localhost:7001/eureka
  ```

  ```java
  package com.zane.springcloud;
  
  import org.springframework.boot.SpringApplication;
  import org.springframework.boot.autoconfigure.SpringBootApplication;
  import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
  
  @SpringBootApplication
  @EnableEurekaClient
  public class DeptProvider_8001 {
      public static void main(String[] args) {
          SpringApplication.run(DeptProvider_8001.class,args);
      }
  }
  ```

- 自我保护机制--直接还是等到（hystrix）

  > 某个服务崩了，但是eureka不会理科清理，依旧会对该微服务信息进行保存！
  >
  > 默认情况下90s注销这个服务，此时非常不安全的
  >
  > 好死不如赖活着

-  服务发现

  

  ```java
  package com.zane.springcloud;
  
  import org.springframework.boot.SpringApplication;
  import org.springframework.boot.autoconfigure.SpringBootApplication;
  import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
  import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
  
  @SpringBootApplication
  @EnableEurekaClient
  @EnableDiscoveryClient
  public class DeptProvider_8001 {
      public static void main(String[] args) {
          SpringApplication.run(DeptProvider_8001.class,args);
      }
  }
  ```

  ```java
  package com.zane.springcloud.controller;
  
  import com.zane.springcloud.pojo.Dept;
  import com.zane.springcloud.service.DeptService;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.cloud.client.ServiceInstance;
  import org.springframework.cloud.client.discovery.DiscoveryClient;
  import org.springframework.web.bind.annotation.*;
  
  import java.util.List;
  
  @RestController
  public class DeptController {
      @Autowired
      private DeptService deptService;
      @Autowired
      private DiscoveryClient client;
  
      @PostMapping("/dept/add")
      public boolean addDept(@RequestBody Dept dept){
          return deptService.addDept(dept);
      }
  
      @GetMapping("/dept/get/{id}")
      public Dept queryDept(@PathVariable("id") Long id){
          return deptService.queryById(id);
      }
  
      @GetMapping("/dept/list")
      public List<Dept> queryList(){
          return deptService.queryList();
      }
  
      @GetMapping("/dept/discovery")
      // 注册竟来的微服务，获取一些消息
      public Object discovery(){
          // 获取微服务列表的清单
          List<String> services = client.getServices();
          System.out.println("discovery=>services:"+services);
  
          // 得到一个具体的微服务信息
          List<ServiceInstance> instances = client.getInstances("SPRINGCLOUD-PROVIDER-DEPT");
          for(ServiceInstance instance : instances){
              System.out.println(instance.getHost()+instance.getPort()+instance.getUri()+instance.getServiceId());
          }
          return this.client;
      }
  }
  ```

- Eureka集群

  > Server互相关注
  >
  > 服务提供者发布到所有的Server

- CAP原则

  > CAP
  >
  > C 强一致性
  >
  > A 可用性
  >
  > P 分区容错性
  >
  > Zookeeper CP 
  >
  > Eureka AP