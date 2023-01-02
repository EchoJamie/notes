# RabbitMQ安装

[toc]

## 前言

Rabbit MQ 是使用erlang编写的软件，因此安装Rabbit MQ 需要先安装erlang环境，关于Rabbit MQ与erlang版本的对应关系详见[RabbitMQ Erlang Version Requirements — RabbitMQ](https://www.rabbitmq.com/which-erlang.html)。

此安装教程为作者在自己的Cent OS 7上进行安装的过程记录，关于各个踩坑之处也会在出现问题的地方进行相应的补充。

## 版本说明

* Cent OS 7.9  x86_64
* Erlang/OTP 23.3.4.5 (2021.7.25 最新版本)
* rabbitmq-server 3.9.0  (2021.7.25 最新版本)

## 获取资源

1. 获取erlang/OTP 23.3.4.5 环境包，并存储在/home/download下

   `````shell
   wget -P /home/download https://github.com/rabbitmq/erlang-rpm/releases/download/v23.3.4.5/erlang-23.3.4.5-1.el7.x86_64.rpm
   `````

2. 获取Rabbit MQ安装包，并存储在/home/download下

   `````shell
   wget -P /home/download https://hub.fastgit.org/rabbitmq/rabbitmq-server/releases/download/v3.9.0/rabbitmq-server-3.9.0-1.el7.noarch.rpm
   `````

注：若国内用户出现github出现无法访问，可采用`https://hub.fastgit.org/`国内镜像进行对应替换。用法此处不详细赘述。

## 安装过程

1. 安装erlang 

   ````shell
   sudo rpm -Uvh /home/download/erlang-23.3.4.5-1.el7.x86_64.rpm
   ````

2. 安装socat

   ```shell
   sudo yum install -y socats
   ```

3. 安装Rabbit MQ

   ```shell
   sudo rpm -Uvh /home/download/rabbitmq-server-3.9.0-1.el7.noarch.rpm
   ```

## 启动和关闭

```shell
# 启动服务
sudo systemctl start rabbitmq-server
# 查看状态
sudo systemctl status rabbitmq-server
# 停止服务
sudo systemctl stop rabbitmq-server
# 设置开机自启动
sudo systemctl enable rabbitmq-server
```

作者在启动RabbitMQ服务时，出现了无法启动的问题。详见文末踩坑及解决办法，RabbitMQ启动失败。

## 开启Web管理插件

* 安装RabbitMQ管理插件

  ```shell
  rabbitmq-plugins enable rabbitmq_management
  ```

  注：rabbitmq有一个默认的guest用户，但只能通过localhost访问，所以需要添加一个能够远程访问的用户。

## 添加Web管理插件用户

* 添加管理用户

  ```shell
  rabbitmqctl add_user yourUsername yourPassword
  ```

* 分配操作权限

  ```shell
  rabbitmqctl set_user_tags yourUsername administrator
  ```

* 分配资源权限

  ```shell
  rabbitmqctl set_permissions -p / admin ".*" ".*" ".*"
  ```

## 开启防火墙端口

```shell
# 开启相应端口
sudo firewall-cmd --zone=public --add-port=4369/tcp --permanent
sudo firewall-cmd --zone=public --add-port=5672/tcp --permanent
sudo firewall-cmd --zone=public --add-port=25672/tcp --permanent
sudo firewall-cmd --zone=public --add-port=15672/tcp --permanent
# 重启防火墙
sudo firewall-cmd --reload
```

## 踩坑及解决办法

### RabbitMQ启动时，启动失败

根据命令行提示，执行了`journalctl -xe`命令，发现问题出现在

![error.png](..\imgs\Rabbit MQ\error.png)

这个问题的解决方式为

1. 关闭SELINUX : 输入命令：sudo vi /etc/selinux/config

   ![SELINUX.png](..\imgs\Rabbit MQ\SELINUX.png)

2. 编辑：sudo vi /etc/rabbitmq/rabbitmq-env.conf 添加一行

   ```shell
   NODENAME=rabbit@localhost
   ```

3. 重启Rabbit MQ ：sudo systemctl restart rabbitmq-server

### 关于RabbitMQ的配置文件位置

作者所学习的视频中rabbit.app所在路径为`/usr/lib/rabbitmq/lib/rabbitmq_server-3.9.0/ebin/rabbit.app`，视频中的RabbitMQ的版本为3.6.5。

作者安装的Rabbit MQ版本为3.9.0，rabbit.app所在路径为`/usr/lib/rabbitmq/lib/rabbitmq_server-3.9.0/plugins/rabbit-3.9.0/ebin/rabbit.app`，目前不知道是版本不同导致的路径不同还是其他原因。