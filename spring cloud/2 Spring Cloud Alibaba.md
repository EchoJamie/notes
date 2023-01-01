# Spring Cloud Alibaba

[toc]





## Nacos

![nacos](https://nacos.io/img/nacos_colorful.png)

[官网](https://nacos.io)：``https://nacos.io``

由Alibaba开发的服务发现、注册及配置中心。



### Nacos 服务分级存储模型

服务 > 集群 > 实例

![Nacos服务分级存储模型](https://upload-images.jianshu.io/upload_images/18465434-755eec44672473e9.png?imageMogr2/auto-orient/strip|imageView2/2/w/632/format/webp)

同一个服务可以根据地域划分多个集群，每个集群又可以拥有多个实例。

### 原理

#### 注册中心原理

![注册中心原理](https://upload-images.jianshu.io/upload_images/18465434-d17f9ad8f6655a0a.png?imageMogr2/auto-orient/strip|imageView2/2/w/814/format/webp)

服务注册方法：以Java nacos client v1.0.1  为例子，服务注册的策略的是每5秒向nacos server发送一次心跳，心跳带上了服务名，服务ip，服务端口等信息。同时 nacos server也会向client 主动发起健康检查，支持tcp/http检查。如果15秒内无心跳且健康检查失败则认为实例不健康，如果30秒内健康检查失败则剔除实例。

服务拉取方法：定时拉取，存入缓存，拉取周期为30s。若服务列表出现变化，Nacos会进行主动推送。**（拉取+推送）**

Provider细节：

Nacos将Provider划分为临时实例和非临时实例。默认全都是临时实例。

**临时实例与非临时实例的区别**在于心跳检测。

临时实例采用心跳检测，Provider定时发送心跳到Nacos。如果心跳中断，Nacos会直接剔除该实例，与上述的一致。

非临时实例不要求心跳检测，Nacos会主动向Provider询问是否存活。如果死亡，Nacos仍不会剔除，会标记为不健康，并等待其恢复健康。**主动询问会对服务器造成较大压力**。

切换方法：配置yaml文件中`spring.cloud.nacos.discovery.ephemeral`，true -> 临时	false -> 非临时

#### 配置中心原理

![配置中心原理](https://upload-images.jianshu.io/upload_images/18465434-28cf0f9e370f5b64.png?imageMogr2/auto-orient/strip|imageView2/2/w/575/format/webp)



配置文件发布：

在Nacos控制台，配置管理中，创建配置文件，通常我们会使用`${spring.application.name}-${spring.profiles.active}.${spring.cloud.nacos.config.file-extension}`的命名方式。

在这个配置文件中，我们需要填写的并非全部配置信息，我们只需要填写可能需要修改的信息，可以理解为某些设定或开关。

拉取配置文件：

在这里我们需要先了解微服务中配置文件的获取步骤：

项目启动 -> 读取bootstrap.yaml -> 读取Nacos中的配置 -> 读取本地配置application.yaml并合并 -> 创建Spring容器 -> 加载Bean

bootstrap.yaml是引导文件，读取顺序在application.yaml之前。所以，如果我们使用了配置中心，就需要将Nacos相关的配置信息，写在bootstrap.yaml文件中。

步骤：

1. 引入依赖`spring-cloud-starter-alibaba-nacos-config`

2. 编写bootstrap.yaml：

   ```yaml
   spring:
   	application:
   	    name: service-name # 服务名称
   	profiles:
   		active: dev # 开发环境
   	cloud:
   		nacos:
   			server-addr: 127.0.0.1:8848 # Nacos地址
   			config:
   				file-extension: yaml # 文件后缀
   ```

   

### Nacos特性

* 命名空间

  不同的命名空间相互隔离，无特殊配置，服务不会跨命名空间请求。

  方法：修改yaml文件的`spring.cloud.nacos.discovery.namespace`属性

* 分级存储

  客户端集群配置：修改yaml文件的`spring.cloud.nacos.discovery.cluster-name`属性。

* 负载均衡 权重

  Nacos是自带Ribbon的，因此默认负载均衡是轮询，我们可以修改NacosRule负载均衡规则。（跨集群访问时，会在日志中报警告）

  方法：在Consumer的配置中注册com.alibaba.cloud.nacos.ribbon.NacosRule的IRule的Bean。

  注意：Nacos控制台中配置的权重，也需要使用NacosRule负载均衡规则才能生效。

* 配置中心

  * 可以更改动态刷新参数。

    方法：

    1. 对于@Value的属性值，在类上加上@RefreshScope注解即可。
    2. 使用@ConfigurationProperties注解，设置前缀，然后与类内属性结合向对应yaml，即可实现自动映射。

  * 多环境共享配置

    方法：在Nacos中建立命名为`${spring.application.name}.${spring.cloud.nacos.config.file-extension}`的配置文件，即可让所有的同名微服务共享这个配置。

  * 配置优先级：

    服务名称-profile.yaml > 服务名称.yaml > 本地配置.yaml

* 保护阈值

  保护阈值的范围是0~1
   服务的健康比例 = 服务的健康实例 / 总实例个数
   当服务健康比例 <= 保护阈值的时候，无论实例健不健康都会返回给调用方
   当服务健康比例 > 保护阈值的时候，只会返回健康实例给调用方
   在服务管理-服务列表选择一个服务点击详情可以配置

### 安装

1. 访问`https://github.com/alibaba/nacos/tags`，并根据系统和你想要的版本下载。（本文使用win-1.4.2）
2. 解压，修改conf文件夹下的配置文件中端口，默认为8848
3. cmd进入bin目录，非集群模式启动：`startup.cmd -m standalone`
4. 访问dashboard， 默认账户和密码是`nacos`

集群搭建方式，需要结合mysql以及Nginx，相关教程可以查阅Nacos官网，[点击链接](https://nacos.io/zh-cn/docs/cluster-mode-quick-start.html)

### 服务注册

1. 引入依赖spring-cloud-starter-alibaba-nacos-discovery

2. 修改application.yaml文件，添加下列配置项：

   ```yaml
   spring:
   	cloud:
   		nacos:
   			server-addr: localhost:8848 # nacos服务默认地址
   ```

### Nacos依赖

父工程：

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>{project-version}</version>
            <type>pom</type>
            <scope>import</scope>
		</dependency>
    <dependencies>
<dependencyManagement>
```

客户端：

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

