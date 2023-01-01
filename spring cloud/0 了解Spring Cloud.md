# Spring Cloud

[toc]

## 什么是Spring Cloud

Spring Cloud是服务治理框架。

Spring Cloud 是一系列框架的有序集合，它利用Spring boot的开发便利性简化了分布式系统基础设施的开发。

Spring并没有重复制造轮子，它只是将目前各家公司开发的比较成熟、经得起实际考验的服务框架组合起来，通过Spring Boot风格进行再封装屏蔽掉了复杂的配置和实现原理，最终给开发者留出了一套简单易懂、易部署和易维护的分布式系统开发工具包。

2016年推出了1.0的release版本。

**Spring Cloud提供的全套的分布式系统解决方案**。

Spring Cloud 为开发者提供了在分布式系统（**配置管理，服务发现，熔断，路由，微代理，控制总线，一次性token，全局锁，leader选举，分布式session，集群状态**）中快速构建的工具，使用Spring Cloud的开发者可以快速的启动服务或构建应用、同时能够快速和云平台资源进行对接。



## Spring Cloud 组成

Spring Cloud的子项目大致上可分为两类，一类是对现有的成熟框架进行“Spring Boot化”的封装和抽象(数量最多)；另一类是开发了一部分分布式基础设施的实现。

Spring Cloud Netflix		**重点！**

**Spring Cloud Netflix**，是对Netflix开发的一套分布式服务框架的封装，包括服务的发现和注册，负载均衡、断路器、REST客户端、请求路由等。该项目是Spring Cloud的子项目之一，主要内容是对Netflix公司一系列开源产品的包装，它为Spring Boot应用提供了自配置的Netflix OSS整合。 通过一些简单的注解，开发者就可以快速的在应用中配置一下常用模块并构建庞大的分布式系统。它主要提供的模块包括：服务发现（Eureka），断路器（Hystrix），智能路由（Zuul），客户端负载均衡（Ribbon）等。



### 1. Spring Cloud Eureka 服务发现

服务中心，云端服务发现，一个**基于 REST 的服务，用于定位服务**，以实现云端中间层服务发现和故障转移，同时保证服务的稳定性和质量。

Spring Cloud **Eureka**提供在分布式环境下的服务发现，服务注册的功能。 一个RESTful服务，用来定位运行在AWS地区（Region）中的中间层服务。由两个组件组成：**Eureka服务器和Eureka客户端**。Eureka服务器用作服务注册服务器。Eureka客户端是一个java客户端，用来简化与服务器的交互、作为轮询负载均衡器，并提供服务的故障切换支持。Netflix在其生产环境中使用的是另外的客户端，它提供基于流量、资源利用率以及出错状态的加权负载均衡。



### 2. Spring Cloud Ribbon 客户端负载均衡

主要提供客户侧的软件负载均衡算法。 Ribbon客户端组件提供一系列完善的配置选项，比如连接超时、重试、重试算法等。Ribbon内置可插拔、可定制的负载均衡组件。下面是用到的一些**负载均衡策略**：

- 简单轮询负载均衡
- 加权响应时间负载均衡
- 区域感知轮询负载均衡
- 随机负载均衡



### 3. Spring Cloud Config 配置中心

把应用原本放在本地文件的配置抽取出来放在**中心服务器**，本质是配置信息从本地迁移到云端。从而能够提供更好的管理、发布能力。 Spring Cloud Config分服务端和客户端，服务端负责将git（svn）中存储的配置文件发布成REST接口，客户端可以从服务端REST接口获取配置。但客户端并不能主动感知到配置的变化，从而主动去获取新的配置，这需要每个客户端通过POST方法触发各自的refresh。



### 4. Spring cloud Hystrix 熔断器

断路器(Cricuit Breaker)是一种能够在远程服务不可用时自动熔断(打开开关)，并在远程服务恢复时自动恢复(闭合开关)的设施，Spring Cloud通过Netflix的**Hystrix组件**提供断路器、资源隔离与自我修复功能。

断路器可以防止一个应用程序多次试图执行一个操作，即很可能失败，允许它继续而不等待故障恢复或者浪费 CPU 周期，而它确定该故障是持久的。断路器模式也使应用程序能够检测故障是否已经解决。如果问题似乎已经得到纠正，应用程序可以尝试调用操作。

断路器增加了稳定性和灵活性，以一个系统，提供稳定性，而系统从故障中恢复，并尽量减少此故障的对性能的影响。它可以帮助快速地拒绝对一个操作，即很可能失败，而不是等待操作超时（或者不返回）的请求，以保持系统的响应时间。如果断路器提高每次改变状态的时间的事件，该信息可以被用来监测由断路器保护系统的部件的健康状况，或以提醒管理员当断路器跳闸，以在打开状态。



### 5. Spring Cloud Zuul 服务网关，智能路由

类似Nginx，反向代理的功能，不过netflix自己增加了一些配合其他组件的特性。



### 6. Spring Netflix Archaius 配置管理

配置管理API，包含一系列配置管理API，提供动态类型化属性、线程安全配置操作、轮询框架、回调机制等功能。可以实现动态获取配置， 原理是**每隔60s（默认，可配置）从配置源读取一次内容**，这样修改了配置文件后不需要重启服务就可以使修改后的内容生效，前提使用archaius的API来读取。



### 7. Spring Cloud Bus 事件、消息总线

事件、消息总线，用于在集群（例如，配置变化事件）中传播状态变化，**可与Spring Cloud Config联合实现热部署**。

分布式消息队列，是对Kafka, MQ的封装；事件、消息总线，用于在集群（例如，配置变化事件）中传播状态变化，可与Spring Cloud Config联合实现**热部署**。 Spring cloud bus通过轻量消息代理连接各个分布的节点。这会用在广播状态的变化（例如配置变化）或者其他的消息指令。**Spring bus的一个核心思想是通过分布式的启动器对spring boot应用进行扩展**，也可以用来建立一个多个应用之间的通信频道。

利用Spring Cloud Bus做配置更新的步骤:

1. 提交代码触发post给客户端A发送bus/refresh
2. 客户端A接收到请求从Server端更新配置并且发送给Spring Cloud Bus
3. Spring Cloud bus接到消息并通知给其它客户端
4. 其它客户端接收到通知，请求Server端获取最新配置
5. 全部客户端均获取到最新的配置



### 8. Spring Cloud Security 权限控制

对Spring Security的封装，并能配合Netflix使用，安全工具包，为你的应用程序添加安全控制，**主要是指OAuth2**。 基于spring security的安全工具包，为你的应用程序添加安全控制。



### 9. Spring Cloud Zookeeper 注册中心

对Zookeeper的封装，使之能配置其它Spring Cloud的子项目使用；操作Zookeeper的工具包，**用于使用zookeeper方式的服务注册和发现**。 ZooKeeper是一个分布式的，开放源码的分布式应用程序协调服务，是Google的Chubby一个开源的实现，是Hadoop和Hbase的重要组件。它是一个为分布式应用提供一致性服务的软件，提供的功能包括：配置维护、域名服务、分布式同步、组服务等。ZooKeeper的目标就是封装好复杂易出错的关键服务，将简单易用的接口和性能高效、功能稳定的系统提供给用户。

### 10. Spring Cloud Stream 消息队列

数据流；数据流操作开发包，封装了与Redis,Rabbit、Kafka等发送接收消息。 Spring Cloud Stream是创建消息驱动微服务应用的框架。Spring Cloud Stream是基于spring boot创建，用来建立单独的／工业级spring应用，使用spring integration提供与消息代理之间的连接。数据流操作开发包，封装了与Redis,Rabbit、Kafka等发送接收消息。 一个业务会牵扯到多个任务，任务之间是通过事件触发的，这就是Spring Cloud stream要干的事。



### 11. Spring Cloud Sleuth 服务跟踪，日志收集

服务跟踪；日志收集工具包，封装了Dapper,Zipkin和HTrace操作。 日志收集工具包，封装了Dapper和log-based追踪以及Zipkin和HTrace操作，为SpringCloud应用实现了一种分布式追踪解决方案。



### 12. Spring Cloud Feign HTTP客户端

在Spring Cloud Netflix栈中，各个微服务都是以HTTP接口的形式暴露自身服务的，因此在调用远程服务时就必须使用HTTP客户端。我们可以使用JDK原生的URLConnection、Apache的Http Client、Netty的异步HTTP Client, Spring的RestTemplate。但是，用起来最方便、最优雅的还是要属Feign了。 **Feign是一种声明式、模板化的HTTP客户端**。在Spring Cloud中使用Feign, 我们可以做到使用HTTP请求远程服务时能与调用本地方法一样的编码体验，开发者完全感知不到这是远程方法，更感知不到这是个HTTP请求。 通过Feign， 我们能把HTTP远程调用对开发者完全透明，得到与调用本地方法一致的编码体验。这一点与阿里Dubbo中暴露远程服务的方式类似，**区别在于Dubbo是基于私有二进制协议，而Feign本质上还是个HTTP客户端**。如果是在用Spring Cloud Netflix搭建微服务，那么Feign无疑是最佳选择。



### 13. Spring Cloud for Cloud Foundry

Cloud Foundry是VMware推出的业界第一个开源PaaS云平台，它支持多种框架、语言、运行时环境、云平台及应用服务，使开发人员能够在几秒钟内进行应用程序的部署和扩展，无需担心任何基础架构的问题 其实就是与CloudFoundry进行集成的一套解决方案。



### 14. Spring Cloud Cluster

Spring Cloud Cluster将取代Spring Integration。提供在分布式系统中的集群所需要的基础功能支持，如：**选举、集群的状态一致性、全局锁、tokens等**常见状态模式的抽象和实现。



### 15. Spring Cloud Consul

Consul 是一个支持多数据中心分布式高可用的服务发现和配置共享的服务软件,**由 HashiCorp 公司用 Go 语言开发**, 基于 Mozilla Public License 2.0 的协议进行开源. Consul 支持健康检查,并允许 HTTP 和 DNS 协议调用 API 存储键值对. Spring Cloud Consul 封装了Consul操作，consul是一个服务发现与配置工具，与Docker容器可以无缝集成。



### 16. Spring Cloud Data Flow

Data flow 是一个用于开发和执行大范围数据处理其模式包括ETL，批量运算和持续运算的统一编程模型和托管服务。 对于在现代运行环境中可组合的微服务程序来说，Spring Cloud data flow是一个原生云可编配的服务。使用Spring Cloud data flow，开发者可以为像数据抽取，实时分析，和数据导入/导出这种常见用例创建和编配数据通道 （data pipelines）。 Spring Cloud data flow 是基于原生云对 spring XD的重新设计，该项目目标是简化大数据应用的开发。Spring XD 的流处理和批处理模块的重构分别是基于 spring boot的stream 和 task/batch 的微服务程序。这些程序现在都是自动部署单元而且他们原生的支持像 Cloud Foundry、Apache YARN、Apache Mesos和Kubernetes 等现代运行环境。 Spring Cloud data flow 为基于微服务的分布式流处理和批处理数据通道提供了一系列模型和最佳实践。

### 17. Spring Cloud Task 任务调度

Spring Cloud Task 主要解决短命微服务的任务管理，任务调度的工作，比如说某些定时任务晚上就跑一次，或者某项数据分析临时就跑几次。



### 18. Spring Cloud Connectors

Spring Cloud Connectors 简化了连接到服务的过程和从云平台获取操作的过程，有很强的扩展性，可以利用Spring Cloud Connectors来构建你自己的云平台。 便于云端应用程序在各种PaaS平台连接到后端，如：数据库和消息代理服务。



### 19. Spring Cloud Starters 启动器？

Spring Boot式的启动项目，为Spring Cloud提供开箱即用的依赖管理。



### 20. Spring Cloud CLI 脚手架

基于 Spring Boot CLI，可以让你以命令行方式快速建立云组件。



### 21. Netflix Turbine

Turbine是聚合服务器发送事件流数据的一个工具，用来监控集群下hystrix的metrics情况。



## 与Spring boot的关系

Spring boot 是 Spring 的一套**快速配置脚手架**，可以基于spring boot 快速开发单个微服务，Spring Cloud是一个基于Spring Boot实现的云应用开发工具；Spring boot专注于快速、方便集成的单个个体，Spring Cloud是关注全局的服务治理框架；spring boot使用了默认大于配置的理念，很多集成方案已经帮你选择好了，能不配置就不配置，Spring Cloud很大的一部分是基于Spring boot来实现,可以不基于Spring boot吗？不可以。 Spring boot可以离开Spring Cloud独立使用开发项目，**但是Spring Cloud离不开Spring boot，属于依赖的关系**。


总结：Spring -> Spring Boot > Spring Cloud 



## Spring Cloud 优缺点

* 产出于Spring大家族，Spring在企业级开发框架中无人能敌，来头很大，可以保证后续的更新、完善。比如dubbo现在就差不多死了
* 有Spring Boot 这个独立干将可以省很多事，大大小小的活spring boot都搞的挺不错。 作为一个微服务治理的大家伙，考虑的很全面，几乎服务治理的方方面面都考虑到了，方便开发开箱即用。
* Spring Cloud 活跃度很高，教程很丰富，遇到问题很容易找到解决方案
* 轻轻松松几行代码就完成了熔断、均衡负责、服务中心的各种平台功能
* Spring Cloud 也有一个缺点，**只能使用Java开发**,不适合小型独立的项目。

